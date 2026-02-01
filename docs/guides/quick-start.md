# Quick Start Guide

Get connected to your remote services in 5 minutes.

## Prerequisites

- A SocketZero receiver URL from your organization (looks like `receiver.example.com`)
- Authentication credentials (provided by your IT admin)

## Step 1: Download SocketZero

Download the latest version for your platform:

| Platform | Download |
|----------|----------|
| macOS (Apple Silicon) | [Download for M1/M2/M3 Mac](https://github.com/radiusmethod/socketzero/releases) |
| macOS (Intel) | [Download for Intel Mac](https://github.com/radiusmethod/socketzero/releases) |
| Windows | [Download for Windows](https://github.com/radiusmethod/socketzero/releases) |
| Linux | [Download for Linux](https://github.com/radiusmethod/socketzero/releases) |

## Step 2: Install

### macOS

1. Open the downloaded `.dmg` file
2. Drag SocketZero to your Applications folder
3. Launch SocketZero from Applications

On first launch, you may need to right-click → Open to bypass Gatekeeper.

### Windows

1. Run the downloaded `.exe` installer
2. Follow the installation wizard
3. Launch SocketZero from the Start menu

### Linux

```bash
# Debian/Ubuntu
sudo dpkg -i socketzero_*.deb

# Fedora/RHEL
sudo rpm -i socketzero_*.rpm

# Run
socketzero
```

## Step 3: Add Your First Profile

1. Click **"Add Profile"** in the SocketZero app
2. Enter your receiver hostname (e.g., `receiver.example.com`)
3. Give it a friendly label (e.g., "Work Network")
4. Click **"Add"**

## Step 4: Connect

1. Select your profile from the sidebar
2. Click **"Connect"**
3. Authenticate when prompted (your browser will open)
4. Once connected, you'll see available services as tiles

## Step 5: Use a Service

Click on any service tile to establish a tunnel. The connection details will appear (e.g., `ssh myservice.local`).

### Example: SSH Access

If you see a service called "Dev Server" with `ssh dev.internal`:

```bash
ssh dev.internal
```

SocketZero creates the tunnel automatically. You connect as if the service was local.

## What's Next?

- [User Guide](user-guide.md) - Learn about profiles, tunnels, and advanced features
- [FAQ](../faq.md) - Common questions and answers
- [Troubleshooting](../troubleshooting.md) - Fix connection issues

## Need Help?

- Check the [FAQ](../faq.md)
- [Open an issue](https://github.com/radiusmethod/socketzero-docs/issues)
- Email: support@radiusmethod.com
