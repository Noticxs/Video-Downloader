# 🐳 Video Downloader - CasaOS Installation Guide

This guide will help you install the Video Downloader on CasaOS using Docker Compose.

## 📋 Prerequisites

- CasaOS installed and running
- Docker and Docker Compose available on your system
- Basic familiarity with CasaOS interface

## 🚀 Quick Installation

### Method 1: Using CasaOS App Store (Recommended)

1. **Open CasaOS** in your web browser
2. **Go to App Store**
3. **Click "Install a customized app"**
4. **Paste this Docker Compose configuration:**

```yaml
version: '3.8'

services:
  video-downloader:
    build: .
    container_name: video-downloader
    restart: unless-stopped
    ports:
      - "2070:2070"
    volumes:
      - ./downloads:/app/music
      - ./config:/app/config
    environment:
      - FLASK_ENV=production
      - PYTHONUNBUFFERED=1
      - FLASK_HOST=0.0.0.0
      - FLASK_PORT=2070
    network_mode: "bridge"
    labels:
      # CasaOS specific labels for better integration
      - "casaos.name=Video Downloader"
      - "casaos.icon=🎬"
      - "casaos.description=Modern web-based video downloader with real-time progress tracking"
      - "casaos.category=Media"
      - "casaos.port_map=2070"
      - "casaos.web_ui=true"
      - "casaos.web_ui_port=2070"
      - "casaos.web_ui_path=/"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:2070/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

volumes:
  downloads:
    driver: local
  config:
    driver: local
```

5. **Click Install**

### Method 2: Manual Installation

1. **SSH into your CasaOS system** or use the terminal
2. **Create a new directory:**
   ```bash
   mkdir ~/video-downloader
   cd ~/video-downloader
   ```

3. **Create the required files:**
   ```bash
   # Create docker-compose.yml (copy content from above)
   nano docker-compose.yml
   
   # Create Dockerfile
   nano Dockerfile
   
   # Create requirements.txt
   nano requirements.txt
   
   # Copy your app.py file
   nano app.py
   ```

4. **Start the application:**
   ```bash
   docker-compose up -d
   ```

## 📁 File Structure

Your project directory should look like this:
```
video-downloader/
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── app.py
├── downloads/          (created automatically)
└── config/            (created automatically)
```

## ⚙️ Configuration

### Port Configuration
- **Default Port**: 2070
- **To change port**: Edit the `ports` section in `docker-compose.yml`
  ```yaml
  ports:
    - "YOUR_PORT:2070"
  ```

### Volume Mapping
- **Downloads**: `./downloads:/app/music`
- **Config**: `./config:/app/config`

### Environment Variables
You can customize the application by adding environment variables:
```yaml
environment:
  - FLASK_ENV=production
  - PYTHONUNBUFFERED=1
  - CUSTOM_DOWNLOAD_PATH=/app/music
```

## 🔧 CasaOS Integration Features

### App Labels
The Docker Compose includes CasaOS-specific labels:
- **Name**: Video Downloader
- **Icon**: 🎬
- **Category**: Media
- **Web UI**: Enabled on port 2070

### Health Monitoring
- **Health Check**: Automated monitoring
- **Auto-restart**: Container restarts if it fails
- **Status**: Visible in CasaOS dashboard

## 🛠️ Management Commands

### View Logs
```bash
docker-compose logs -f video-downloader
```

### Restart Service
```bash
docker-compose restart video-downloader
```

### Update Application
```bash
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

### Stop Service
```bash
docker-compose down
```

## 📱 Accessing the Application

Once installed, you can access Video Downloader:

1. **Through CasaOS Dashboard**: Look for the Video Downloader tile
2. **Direct URL**: `http://YOUR_CASAOS_IP:2070`
3. **Local Access**: `http://localhost:2070`

## 🔒 Security Considerations

### User Permissions
- Runs as non-root user (UID 1000)
- Limited system access
- Isolated network environment

### Data Protection
- Downloads stored in mapped volume
- Config files persistent across updates
- No sensitive data in container

## 🐛 Troubleshooting

### Container Won't Start
```bash
# Check logs
docker logs video-downloader

# Check port conflicts
netstat -tlnp | grep 2070

# Rebuild container
docker-compose build --no-cache
```

### Permission Issues
```bash
# Fix ownership
sudo chown -R 1000:1000 ./downloads ./config
```

### FFmpeg Issues
The Docker image includes FFmpeg, but if you encounter issues:
```bash
# Rebuild with latest base image
docker-compose build --no-cache --pull
```

## 🔄 Updates and Maintenance

### Automatic Updates
Add this to your `docker-compose.yml` for automatic updates:
```yaml
services:
  video-downloader:
    # ... other configurations
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
```

### Backup Configuration
```bash
# Backup your setup
tar -czf video-downloader-backup.tar.gz docker-compose.yml app.py downloads/ config/
```

## 📊 Resource Usage

### System Requirements
- **RAM**: ~100-200MB
- **Storage**: Depends on downloads
- **CPU**: Low usage, spikes during downloads

### Performance Optimization
- Use SSD for download directory
- Ensure adequate network bandwidth
- Monitor disk space regularly

## 🆘 Support

If you encounter issues:
1. Check the logs: `docker-compose logs -f`
2. Verify file permissions
3. Ensure all required files are present
4. Check CasaOS documentation
5. Restart the service: `docker-compose restart`

---

**Enjoy your new Video Downloader on CasaOS! 🎉**
