# ntfy.sh for GitHub Codespaces

A simple setup to run [ntfy.sh](https://ntfy.sh) in GitHub Codespaces using Docker Compose.

## Quick Start

1. Open this repository in GitHub Codespaces
2. Create the necessary directories:
   ```bash
   mkdir -p cache config
   ```
3. Start the ntfy server:
   ```bash
   docker-compose up -d
   ```
4. Access the web interface at `http://localhost:8080`

## Configuration

The ntfy server configuration is stored in `config/server.yml`. All options are commented out by default, allowing you to uncomment and modify them as needed.

To apply configuration changes:
1. Edit `config/server.yml`
2. Restart the container: `docker-compose restart`

## Key Changes from Default Setup

- **Port**: Changed from port 80 to 8080 to avoid permission issues in Codespaces
- **Volumes**: Uses relative paths (`./cache` and `./config`) instead of absolute paths
- **User**: Removed the `user: UID:GID` directive to avoid permission issues
- **Persistence**: Cache and config directories are mapped as volumes for persistence

## Testing the Setup

Once running, you can test the ntfy server:

1. **Send a notification via curl:**
   ```bash
   curl -d "Hello from Codespaces!" http://localhost:8080/mytopic
   ```

2. **Subscribe to a topic:**
   ```bash
   curl -s http://localhost:8080/mytopic/json
   ```

3. **Use the web interface:**
   Open `http://localhost:8080` in your browser to access the ntfy web app.

## Stopping the Service

```bash
docker-compose down
```

## Logs

To view logs:
```bash
docker-compose logs -f ntfy
```

## iOS Push Notifications

For iOS users using the ntfy iPhone app, you **must** configure the upstream server to receive instant push notifications. This is already configured in the `config/server.yml` file:

```yaml
upstream-base-url: "https://ntfy.sh"
```

This setting tells your self-hosted server to use the official ntfy.sh server for iOS/Firebase push notifications while keeping your message content private on your own server.

After making this change, restart the container:
```bash
docker-compose restart
```

> Note that there are some known issues around iOS Push notifications mentioned [here](https://docs.ntfy.sh/known-issues/)

## Customization

You can customize the ntfy server by editing `config/server.yml`. Some common configurations:

- **iOS Push Notifications**: Already configured with `upstream-base-url: "https://ntfy.sh"`
- **Enable authentication**: Uncomment and configure `auth-file` and related settings
- **Set base URL**: Configure `base-url` for your Codespace URL (needed for attachments and emails)
- **Adjust rate limits**: Modify visitor limits as needed
- **Enable features**: Turn on signup, login, or other features

Refer to the [official ntfy documentation](https://ntfy.sh/docs/config/) for detailed configuration options.