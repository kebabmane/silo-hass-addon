# Silo Feed Reader for Home Assistant

## About

Silo is a self-hosted RSS/Atom feed reader with AI-powered daily briefs. It provides a clean, modern interface for reading your favorite feeds directly within Home Assistant.

## Features

- Clean, distraction-free reading interface
- AI-powered daily brief summaries (configure in app settings)
- Keyboard shortcuts for power users
- OPML import/export for easy migration
- Dark mode support
- Mobile-friendly progressive web app (PWA)
- Full-text search across all articles
- **Zero configuration required** - admin credentials auto-generated

## Installation

1. Add this repository to your Home Assistant Add-on Store
2. Install the Silo add-on
3. Start the add-on
4. Check the logs for your admin credentials
5. Access Silo via the sidebar

## First-Time Setup

On first startup, Silo automatically:

1. Generates all required security keys
2. Creates an admin user with a random password
3. Displays the credentials in the add-on logs

**Finding Your Admin Credentials:**

1. Go to **Settings > Add-ons > Silo > Logs**
2. Look for the "SILO ADMIN ACCESS" section
3. Note the email and password

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SILO ADMIN ACCESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Email: admin@silo.local
Password: [your-generated-password]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Change this password after first login!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

4. Click "Silo" in the sidebar and log in

## Configuration

All internal settings (security keys, database, etc.) are automatically managed. You only need to configure optional user preferences.

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

## Data Storage

All data is persisted in Home Assistant's add-on data directory:

- **Database files**: `/data/db/` - SQLite databases for feeds, articles, and settings
- **File uploads**: `/data/storage/` - Any uploaded files or cached content
- **Credentials**: `/data/admin_credentials.json` - Auto-generated admin password

This data is automatically included in Home Assistant backups.

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

1. View the add-on logs for specific error messages
2. Ensure you have enough disk space for the database
3. Try restarting the add-on

### Forgot admin password

The password is displayed in the logs every time the add-on starts. Check **Settings > Add-ons > Silo > Logs**.

To reset credentials completely:
1. Stop the add-on
2. Delete `/data/admin_credentials.json` via SSH/terminal
3. Start the add-on - a new password will be generated

### Database errors

The add-on automatically runs database migrations on startup. If you see migration errors:

1. Check the logs for specific error messages
2. Try restarting the add-on
3. If problems persist, you may need to reset the database (backup first!)

### Daily briefs not generating

1. Configure LiteLLM settings in the app (Admin > Settings)
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
