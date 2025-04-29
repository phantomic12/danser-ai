# Danser Discord Bot

A Discord bot that listens for `.osr` replay files in a specified channel and triggers a GitHub Action to render the replay using [danser-go](https://github.com/Wieku/danser-go).

## Features

- Monitors a specific Discord channel for `.osr` file uploads
- Uploads the replay file to a GitHub repository
- Triggers a GitHub Action workflow to render the replay using danser-go
- Uploads the rendered video to YouTube using Selenium
- Posts a link to the YouTube video back to Discord
- Works in headless server environments with virtual display support

## Setup

### Prerequisites

- Discord Bot Token (create one at [Discord Developer Portal](https://discord.com/developers/applications))
- GitHub Personal Access Token with `repo` and `workflow` scopes
- A GitHub repository to store the replays and run the renderer
- A private repository to store Firefox profile with YouTube login credentials

### Configuration

1. Clone this repository
2. Create a `.env` file in the root directory with the following content:
```
# Discord configuration
DISCORD_TOKEN=your_discord_bot_token
CHANNEL_ID=your_discord_channel_id

# GitHub configuration
GITHUB_TOKEN=your_github_personal_access_token
GITHUB_REPO=username/repo-name
WORKFLOW_ID=render-replay.yml

# Upload configuration (optional)
# Can be 'discord', 'youtube', or 'both'
UPLOAD_DESTINATION=youtube
```

3. Add the following secrets to your GitHub repository:
   - `DISCORD_WEBHOOK_URL`: A Discord webhook URL to post the rendered videos
   - `DISCORD_SERVER_ID`: Your Discord server ID
   - `FIREFOX_PROFILE_REPO`: Private repository containing Firefox profile
   - `FIREFOX_PROFILE_TOKEN`: Token with access to the Firefox profile repository

### Firefox Profile Setup for YouTube

To authenticate with YouTube without API limits:

1. Create a private repository to store your Firefox profile
2. Set up Firefox and log in to your YouTube account
3. Copy your Firefox profile directory to the repository
   - On Windows: `%APPDATA%\Mozilla\Firefox\Profiles\<profile folder>`
   - On macOS: `~/Library/Application Support/Firefox/Profiles/<profile folder>`
   - On Linux: `~/.mozilla/firefox/<profile folder>`
4. Create a directory called `firefox_profile` in your repository and copy the profile contents there
5. Push the repository to GitHub (ensure it is private to protect your credentials)

### Installation

#### Using Docker (recommended)

```bash
# Build and run the container
docker-compose up -d
```

#### Manual Installation

```bash
# Install dependencies
npm install

# Start the bot
npm start
```

## Usage

1. Invite your bot to your Discord server with proper permissions (Read Messages, Send Messages, Read Message History, Attach Files)
2. Upload an `.osr` file to the configured channel
3. The bot will acknowledge the file and start the rendering process
4. When rendering completes, the video will be uploaded to YouTube and a link will be shared in Discord

## GitHub Action Workflow

The included GitHub Action workflow:

1. Downloads and sets up danser-go
2. Creates a virtual display using Xvfb to handle headless server rendering
3. Renders the replay to a video file using the virtual display
4. Uploads the rendered video as a GitHub artifact
5. Uses Selenium to upload the video to YouTube (bypassing API limits)
6. Sends a notification with YouTube link to Discord

## Technical Notes

- **Headless Server Support**: This setup uses Xvfb (X Virtual Framebuffer) to create a virtual display, allowing danser-go to run on headless servers without a physical display. We also enable software rendering as a fallback using `LIBGL_ALWAYS_SOFTWARE=1` if hardware rendering isn't available.

## Credits

- This project uses [youtube-uploader-selenium](https://github.com/linouk23/youtube_uploader_selenium) for YouTube uploads
- [danser-go](https://github.com/Wieku/danser-go) for rendering osu! replays

## License

MIT