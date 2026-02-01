# Frequently Asked Questions

## General

### What is SocketZero?

SocketZero is a secure remote access tool that creates encrypted tunnels from your local machine to services in remote networks. Unlike VPNs, SocketZero uses a Zero Trust model where each service requires explicit authorization.

### How is SocketZero different from a VPN?

| Feature | SocketZero | Traditional VPN |
|---------|------------|-----------------|
| Access Model | Zero Trust (per-service) | Network-wide access |
| Performance | Direct tunnels, low latency | Routes all traffic through VPN |
| Security | Fine-grained per service | Broad network access |
| Setup | Add profile, connect | Install cert, connect to network |

### Is SocketZero open source?

No, SocketZero is closed-source software. However, this documentation is public to help users.

### What platforms does SocketZero support?

- macOS (Apple Silicon and Intel)
- Windows (64-bit)
- Linux (Debian, Ubuntu, Fedora, RHEL)

## Installation

### Do I need admin/root access to install?

- **macOS:** No (drag to Applications)
- **Windows:** Yes (installer requires elevation)
- **Linux:** Yes (package managers require sudo)

### Can I install SocketZero without internet access?

Yes, download the installer on a machine with internet, then transfer it via USB or shared drive.

### Why does macOS say SocketZero "cannot be opened"?

This is Gatekeeper protecting you from unsigned apps. To bypass:

1. Right-click SocketZero in Applications
2. Click "Open"
3. Confirm you want to open it

You only need to do this once.

## Connection Issues

### "Connection refused" error

**Causes:**
- Receiver is offline or unreachable
- Firewall blocking port 9997 (default receiver port)
- Wrong hostname in profile

**Solutions:**
1. Verify the receiver hostname with your IT admin
2. Check if you can reach the receiver: `ping receiver.example.com`
3. Try with the port explicit: `receiver.example.com:9997`

### "Authentication failed" error

**Causes:**
- Invalid credentials
- Session expired
- Identity provider configuration changed

**Solutions:**
1. Click "Logout" then "Connect" to re-authenticate
2. Clear your browser cookies for the auth domain
3. Contact your IT admin to verify your account access

### Connection drops frequently

**Causes:**
- Unstable network connection
- Receiver restarting
- Firewall interfering with WebSocket connections

**Solutions:**
1. Check your network stability
2. Enable auto-reconnect (on by default)
3. Contact your IT admin about receiver health

### "No services available" after connecting

**Causes:**
- Your user/role doesn't have access to any services
- Services are offline
- Authorization rules changed

**Solutions:**
1. Contact your IT admin to verify your permissions
2. Check if other users can see services
3. Try disconnecting and reconnecting

## Using Services

### How do I use an SSH service?

1. Click the SSH service tile in SocketZero
2. Open your terminal
3. Run: `ssh username@service-hostname`

The hostname (e.g., `dev.internal`) is created by SocketZero.

### Can I use multiple services at once?

Yes! Each service tile creates an independent tunnel. You can have as many active as needed.

### Why can't I connect to a service even though the tunnel is open?

**Common causes:**
- Wrong credentials for the service itself (separate from SocketZero auth)
- Service is down on the remote end
- Firewall rules on the remote service blocking your connection

**Test the tunnel:**
```bash
nc -zv service-hostname port
```

If this fails, the issue is with the tunnel. If it succeeds, the issue is with service authentication.

### Can I connect to the same receiver from multiple devices?

Yes! Install SocketZero on each device and add the same profile. Each device maintains its own session.

## Security

### Is my data encrypted?

Yes. All tunnels use **TLS 1.3** encryption. Your traffic is encrypted from your machine to the receiver.

### Can my IT admin see my traffic?

The receiver can log connection metadata (who connected, when, to which services) but **cannot decrypt the tunnel contents** unless they control both the receiver and the service endpoint.

### What happens if I lose my laptop?

**Immediately:**
1. Contact your IT admin to revoke your SocketZero session
2. Change your authentication password

Your IT admin can disable your account, which invalidates all active sessions.

### Does SocketZero store my credentials?

No. SocketZero uses OAuth tokens from your identity provider. The token is stored locally and can be cleared by logging out.

## Advanced

### Can I run SocketZero headless (no GUI)?

Not yet. The client requires the GUI. However, you can use the underlying `client service` component directly if you're comfortable with Go development. (See the main SocketZero repository.)

### Can I script SocketZero connections?

Not officially supported. The UI is the primary interface. For automation, consider running the receiver and client service components separately.

### What's the difference between the client UI and client service?

- **Client UI**: Electron app you interact with
- **Client service**: Background Go process that handles tunnels

The UI manages the service. Most users never need to know this distinction.

### Where are logs stored?

- macOS: `~/Library/Logs/SocketZero/`
- Windows: `%APPDATA%\SocketZero\logs\`
- Linux: `~/.config/socketzero/logs/`

Enable debug logging with `SOCKETZERO_DEBUG=1`.

## Still Need Help?

- [Troubleshooting Guide](troubleshooting.md)
- [Open an issue](https://github.com/radiusmethod/socketzero-docs/issues)
- Email: support@radiusmethod.com
