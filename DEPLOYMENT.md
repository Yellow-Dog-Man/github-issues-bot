# GitHub Issues Bot - Deployment Guide

This guide covers multiple deployment options for running your Discord bot permanently on a server with automatic restart capabilities.

## Prerequisites

- Docker
- Your Discord bot token and GitHub credentials configured in `.env` file
- Server with root/sudo access

## Environment Variables

Create a `.env` file in the project root with the following variables:

```env
# Discord Bot Configuration
BOT_TOKEN=your_discord_bot_token_here
GUILD_ID=your_discord_guild_id_here

# GitHub Configuration
GITHUB_ACCESS_TOKEN=your_github_personal_access_token_here
GITHUB_USERNAME=your_github_username
GITHUB_REPOSITORY=your_repository_name
```

## Deployment Options

### Docker Deployment

Docker provides containerized deployment with isolation and easy scaling.

#### Quick Deploy
```bash
# Deploy with Docker
./deploy.sh docker
```

#### Manual Docker Setup
```bash
# Build and start containers
docker compose up --build -d

# View logs
docker compose logs -f

# Stop containers
docker compose down

# Restart containers
docker compose restart
```

#### Docker Management Commands
```bash
# View running containers
docker compose ps

# View logs
docker compose logs -f github-issues-bot

# Restart the bot
docker compose restart github-issues-bot

# Update and restart
docker compose down
docker compose up --build -d
```

## Monitoring and Maintenance

### Health Checks

All deployment methods include health monitoring:

- **PM2**: Automatic restart on crashes, memory monitoring
- **Docker**: Health check every 30 seconds
- **Systemd**: Automatic restart with 10-second delay

### Log Management
- **Docker**: Use `docker compose logs` to view logs

### Updating the Bot

1. Pull latest changes
2. Update dependencies: `npm install`
3. Rebuild: `npm run build`
4. Restart the service:
   - Docker: `docker compose restart`

## Troubleshooting

### Common Issues

1. **Bot not starting:**
   - Check environment variables in `.env`
   - Verify Discord bot token is valid
   - Check GitHub token permissions

2. **Permission errors:**
   - Ensure proper file ownership
   - Check user permissions for systemd

3. **Memory issues:**
   - Monitor memory usage
   - Adjust memory limits in configuration

### Debugging

```bash

# Docker debugging
docker compose logs --tail=100 github-issues-bot
```

## Security Considerations

- Use environment variables for sensitive data
- Run services with minimal privileges
- Keep dependencies updated
- Monitor logs for suspicious activity
- Use firewall rules to restrict access

## Performance Optimization

- Monitor memory usage and adjust limits
- Consider load balancing for multiple instances
- Implement proper logging rotation

## Backup and Recovery

- Backup your `.env` file securely
- Backup Docker volumes if using persistent data
- Document your deployment configuration
