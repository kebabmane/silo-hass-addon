# Silo Feed Reader for Home Assistant

## About

Silo is a self-hosted RSS/Atom feed reader with AI-powered daily briefs. It provides a clean, modern interface for reading your favorite feeds directly within Home Assistant.

## Features

- Clean, distraction-free reading interface
- AI-powered daily brief summaries (requires LiteLLM)
- Keyboard shortcuts for power users
- OPML import/export for easy migration
- Dark mode support
- Mobile-friendly progressive web app (PWA)
- Full-text search across all articles

## Installation

1. Add this repository to your Home Assistant Add-on Store
2. Install the Silo add-on
3. Configure the required settings (see below)
4. Start the add-on
5. Access Silo via the sidebar

## Configuration

### Required Settings

#### Rails Master Key

This is required to decrypt Rails credentials and must be set before the add-on will start.

**For a fresh installation:**

You have two options:

1. **Use the default key** (easiest): The add-on includes a default master key for fresh installations. Simply leave the field empty and a new key will be generated.

2. **Generate your own**: If you're running Silo elsewhere and want to migrate, copy the contents of `config/master.key` from your existing installation.

### Optional Settings

#### Timezone

Set your local timezone for proper date display and daily brief scheduling. Examples:
- `America/New_York`
- `Europe/London`
- `Australia/Sydney`
- `Asia/Tokyo`

#### Log Level

Control logging verbosity:
- `debug` - Detailed logs for troubleshooting
- `info` - Normal operation (default)
- `warn` - Only warnings and errors
- `error` - Only errors
- `fatal` - Only fatal errors

#### Max Threads

Number of web server threads. The default of 5 is suitable for personal use. Increase if you experience slow responses with multiple users.

#### Admin Email

Pre-configure an admin account email. If not set, you'll register through the web interface on first access.

#### LiteLLM Settings (for AI Daily Briefs)

To enable AI-powered daily brief summaries, configure:

- **LiteLLM Base URL**: Your LiteLLM proxy URL (e.g., `http://homeassistant.local:4000`)
- **LiteLLM API Key**: Your API key for authentication
- **LiteLLM Model**: The model to use (e.g., `gpt-4`, `claude-3-sonnet`, `ollama/llama2`)

## Data Storage

All data is persisted in Home Assistant's add-on data directory:

- **Database files**: `/data/db/` - SQLite databases for feeds, articles, and settings
- **File uploads**: `/data/storage/` - Any uploaded files or cached content

This data is automatically included in Home Assistant backups.

## First-Time Setup

1. After starting the add-on, click "Silo" in the sidebar
2. Register a new account (or use the admin email if configured)
3. Start adding feeds via the onboarding wizard or manually

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `j` | Next article |
| `k` | Previous article |
| `m` | Mark as read/unread |
| `s` | Star/unstar |
| `a` | Archive |
| `v` | View original |
| `?` | Show help |

## Troubleshooting

### Add-on won't start

1. Check that `rails_master_key` is set correctly
2. View the add-on logs for specific error messages
3. Ensure you have enough disk space for the database

### Database errors

The add-on automatically runs database migrations on startup. If you see migration errors:

1. Check the logs for specific error messages
2. Try restarting the add-on
3. If problems persist, you may need to reset the database (backup first!)

### Slow performance

1. Increase `rails_max_threads` if you have available memory
2. Consider reducing the number of feeds or enabling less frequent refresh intervals
3. Check system resources in Home Assistant

### Daily briefs not generating

1. Verify LiteLLM settings are correct
2. Test your LiteLLM connection independently
3. Check logs for API errors

## Backup and Restore

The add-on supports Home Assistant's native backup system. Your feeds, articles, read states, and settings are all preserved.

To manually backup:
1. Export your feeds as OPML (Settings > Export OPML)
2. Create a Home Assistant backup

To restore:
1. Restore the Home Assistant backup
2. Start the add-on - your data should be preserved

## API Access

Silo provides a REST API for integration with other services. The API is available at the ingress URL with `/api/v1/` prefix.

Example endpoints:
- `GET /api/v1/health` - Health check
- `GET /api/v1/articles` - List articles
- `GET /api/v1/feeds` - List feeds

Authentication is required for most endpoints. See the main Silo documentation for API details.

## Support

- [Silo Documentation](https://github.com/kebabmane/silo-rss)
- [Report Add-on Issues](https://github.com/kebabmane/silo-hass-addon/issues)
- [Home Assistant Community](https://community.home-assistant.io/)

## License

This add-on is released under the Apache License 2.0.
