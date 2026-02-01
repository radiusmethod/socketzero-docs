# User Guide

Complete guide to using SocketZero.

## Table of Contents

- [Understanding SocketZero](#understanding-socketzero)
- [Profiles](#profiles)
- [Connections](#connections)
- [Services and Tunnels](#services-and-tunnels)
- [Authentication](#authentication)
- [Advanced Features](#advanced-features)

## Understanding SocketZero

SocketZero creates secure tunnels from your local machine to services running in remote networks. Unlike traditional VPNs that give you network-wide access, SocketZero uses a **Zero Trust** model where each service requires explicit authorization.

### Key Concepts

- **Profile** - A saved connection to a SocketZero receiver
- **Receiver** - Server running inside the remote network
- **Service** - A remote application or resource (SSH server, database, web app, etc.)
- **Tunnel** - An encrypted connection from your machine to a specific service

## Profiles

Profiles store your receiver connection details. You can have multiple profiles for different networks (work, staging, production, etc.).

### Creating a Profile

1. Click **"Add Profile"**
2. Fill in:
   - **Hostname**: Your receiver URL (with or without port)
   - **Label**: Friendly name for this profile
3. Click **"Add"**

### Editing a Profile

1. Select the profile in the sidebar
2. Click the edit icon (✏️)
3. Update the label or hostname
4. Click **"Save"**

### Deleting a Profile

1. Select the profile
2. Click the delete icon (🗑️)
3. Confirm deletion

### Reordering Profiles

Drag and drop profiles in the sidebar to change their order.

## Connections

### Connecting to a Receiver

1. Select a profile from the sidebar
2. Click **"Connect"**
3. Authenticate when prompted
4. Wait for the connection to establish

**Status indicators:**
- 🟢 Green: Connected
- 🟡 Yellow: Connecting
- 🔴 Red: Disconnected
- ⚠️ Orange: Authentication required

### Disconnecting

Click **"Disconnect"** in the profile panel. This closes all active tunnels for that profile.

### Auto-Reconnect

SocketZero automatically reconnects if:
- Your network connection drops briefly
- The receiver restarts
- Your laptop wakes from sleep

If reconnection fails, you'll need to click "Connect" manually.

## Services and Tunnels

Once connected, you'll see available services as tiles in the main panel.

### Service Types

SocketZero supports multiple service types:

| Type | Icon | Description | Example |
|------|------|-------------|---------|
| SSH | 🖥️ | SSH access to remote servers | `ssh dev.internal` |
| HTTP | 🌐 | Web applications | Open in browser |
| Database | 🗄️ | MySQL, PostgreSQL, etc. | Connect via client |
| Custom | 🔧 | Any TCP/UDP service | Custom port forwarding |

### Opening a Tunnel

Click on any service tile. The tunnel details will appear:

```
Service: Dev Server
Local:   ssh dev.internal
Status:  Connected
```

### Using a Tunnel

#### SSH Example

```bash
ssh username@dev.internal
```

The hostname `dev.internal` is made available by SocketZero. It resolves to `127.0.0.1` with the tunnel active.

#### HTTP Example

Click the service tile and a browser window opens automatically. The URL will look like:

```
http://myapp.local:8080
```

#### Database Example

Connect using your database client with the tunnel hostname:

```
Host: postgres.internal
Port: 5432
User: your_username
```

### Closing a Tunnel

Tunnels close automatically when:
- You disconnect from the receiver
- You quit SocketZero
- The receiver shuts down the service

You can also close individual tunnels by clicking the "X" icon on the service tile.

## Authentication

SocketZero uses **OAuth 2.0** for authentication. Your organization configures the identity provider (Keycloak, Okta, etc.).

### First-Time Authentication

1. Click "Connect" on a profile
2. Your browser opens to the login page
3. Enter your credentials
4. Grant SocketZero access
5. Return to the app (it detects authentication automatically)

### Session Management

Authentication sessions last 24 hours by default. After expiration, you'll need to re-authenticate.

### Logout

Click **"Logout"** in the profile menu to clear your session. You'll need to re-authenticate on the next connection.

## Advanced Features

### Custom Ports

Some services allow custom local port mapping. Check the service configuration to see if this is available.

### Multiple Concurrent Connections

You can connect to multiple profiles simultaneously. Each profile maintains its own set of tunnels.

### Logging and Debugging

Enable debug mode to see detailed connection logs:

**macOS:**
```bash
export SOCKETZERO_DEBUG=1
./Applications/SocketZero.app/Contents/MacOS/SocketZero
```

**Windows:**
```cmd
set SOCKETZERO_DEBUG=1
socketzero.exe
```

**Linux:**
```bash
SOCKETZERO_DEBUG=1 socketzero
```

Logs are written to:
- macOS: `~/Library/Logs/SocketZero/`
- Windows: `%APPDATA%\SocketZero\logs\`
- Linux: `~/.config/socketzero/logs/`

### Configuration File

Advanced users can edit the SocketZero config directly:

**Location:**
- Default: `~/socketzero.config`
- Override: Set `SOCKETZERO_CONFIG_FILE` environment variable

**Format:** JSON

```json
{
  "profiles": [
    {
      "id": "uuid-here",
      "hostname": "receiver.example.com:9997",
      "label": "Production",
      "index": 0
    }
  ],
  "lastSelectedProfileId": "uuid-here"
}
```

⚠️ **Warning:** Manual edits may be overwritten by the app. Use the UI when possible.

## Need Help?

- [FAQ](../faq.md)
- [Troubleshooting](../troubleshooting.md)
- [Open an issue](https://github.com/radiusmethod/socketzero-docs/issues)
