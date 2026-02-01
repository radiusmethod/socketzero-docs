# Troubleshooting Guide

Common issues and how to fix them.

## Connection Problems

### Cannot Connect to Receiver

**Symptoms:**
- "Connection refused" error
- Red status indicator
- Timeout when clicking "Connect"

**Diagnosis:**

1. **Test basic connectivity:**
   ```bash
   ping receiver.example.com
   ```
   If this fails, you can't reach the receiver at all (DNS or network issue).

2. **Test receiver port:**
   ```bash
   nc -zv receiver.example.com 9997
   ```
   If this fails, the receiver is down or port 9997 is blocked.

3. **Check your profile configuration:**
   - Hostname should match what IT provided
   - Port defaults to 9997 (only add `:port` if different)

**Solutions:**

- **Network issue:** Contact your network admin
- **Receiver down:** Contact your IT admin
- **Wrong hostname:** Update your profile with the correct receiver URL
- **Firewall:** Ask IT to allow outbound connections to port 9997

### Authentication Keeps Failing

**Symptoms:**
- Browser opens but login fails
- "Authentication failed" error after entering credentials
- Stuck on "Authenticating..." screen

**Diagnosis:**

1. **Try authenticating in an incognito/private browser window**
   - If this works, your browser cookies/cache are stale

2. **Check the auth URL in the browser**
   - Should redirect to your identity provider (Okta, Keycloak, etc.)
   - If it 404s or redirects to the wrong place, auth config is broken

3. **Verify your credentials work elsewhere**
   - Try logging into your company's SSO portal with the same credentials

**Solutions:**

- **Stale cookies:** Clear browser cache, or use incognito mode
- **Wrong identity provider:** Contact IT (receiver config may have changed)
- **Account locked:** Contact IT to unlock your account
- **Permissions changed:** Ask IT to verify your access

### Connection Drops Immediately After Connecting

**Symptoms:**
- Green "Connected" → Red "Disconnected" within seconds
- No services appear

**Diagnosis:**

1. **Check logs:**
   - macOS: `~/Library/Logs/SocketZero/`
   - Windows: `%APPDATA%\SocketZero\logs\`
   - Linux: `~/.config/socketzero/logs/`

2. **Look for errors related to:**
   - WebSocket connection failures
   - Certificate validation errors
   - Authorization failures

**Solutions:**

- **Certificate issue:** Your system may not trust the receiver's TLS cert. Contact IT.
- **Authorization:** Your user/role may not be allowed. Contact IT.
- **Receiver restart loop:** Receiver is unstable. Contact IT.

## Service/Tunnel Problems

### Tunnel Opens But Service Unreachable

**Symptoms:**
- Service tile shows "Connected"
- But `ssh service.local` or browser connection fails

**Diagnosis:**

1. **Test the tunnel:**
   ```bash
   nc -zv service.local PORT
   ```
   Replace `PORT` with the service port (e.g., 22 for SSH, 80 for HTTP).

   - **Succeeds:** Tunnel works, issue is with service authentication
   - **Fails:** Tunnel broken or service down

2. **Check SocketZero logs for tunnel errors**

**Solutions:**

- **Tunnel broken:** Disconnect and reconnect
- **Service down:** Contact IT (the remote service is offline)
- **Wrong credentials:** Verify username/password for the service itself (not SocketZero)

### SSH Says "Connection Refused"

**Causes:**
- Tunnel not open
- SSH service down on remote host
- Firewall blocking SSH on remote host

**Diagnosis:**

1. **Verify tunnel is active:**
   - Service tile should show "Connected" in SocketZero

2. **Test SSH port:**
   ```bash
   nc -zv service.local 22
   ```

3. **Try verbose SSH:**
   ```bash
   ssh -vvv username@service.local
   ```
   This shows where the connection fails.

**Solutions:**

- **Tunnel not open:** Click the service tile in SocketZero first
- **SSH down:** Contact IT
- **Firewall:** Ask IT to allow SSH on the remote host

### HTTP Service Won't Open in Browser

**Symptoms:**
- Click service tile
- Browser opens but shows "Can't connect" or timeout

**Diagnosis:**

1. **Check the URL:**
   - Should be something like `http://myapp.local:8080`
   - Verify port matches the service config

2. **Test manually:**
   ```bash
   curl http://myapp.local:8080
   ```

3. **Check browser console for errors**

**Solutions:**

- **Wrong port:** Check service tile for correct port
- **HTTPS required:** Try `https://` instead of `http://`
- **Service down:** Contact IT

## Performance Issues

### Slow Connections

**Symptoms:**
- Tunnel works but is very slow
- High latency

**Diagnosis:**

1. **Test your network speed:**
   ```bash
   ping receiver.example.com
   ```
   High ping times (>100ms) indicate network issues.

2. **Check receiver load:**
   - Contact IT to see if receiver is overloaded

**Solutions:**

- **Network congestion:** Use wired connection instead of Wi-Fi
- **Receiver overloaded:** IT may need to scale up capacity
- **Distance:** If receiver is far away geographically, latency is expected

### High CPU Usage

**Symptoms:**
- SocketZero using significant CPU
- Fan noise / battery drain

**Diagnosis:**

1. **Check how many tunnels are active:**
   - Disconnect from unused services

2. **Look for crash loops in logs**

**Solutions:**

- **Too many tunnels:** Close unused services
- **Crash loop:** Restart SocketZero, check logs, report issue
- **Bug:** Report to SocketZero team with logs

## macOS-Specific Issues

### "SocketZero cannot be opened because the developer cannot be verified"

**Solution:**

1. Right-click SocketZero in Applications
2. Click "Open"
3. Click "Open" in the dialog

OR

```bash
xattr -d com.apple.quarantine /Applications/SocketZero.app
```

### Permission Denied When Launching

**Solution:**

```bash
chmod +x /Applications/SocketZero.app/Contents/MacOS/SocketZero
```

## Windows-Specific Issues

### Windows Defender Blocks SocketZero

**Solution:**

1. Open Windows Security
2. Go to "Virus & threat protection"
3. Click "Manage settings"
4. Add SocketZero to exclusions

### Installer Fails with "Administrator required"

**Solution:**

Right-click the installer and choose "Run as administrator".

## Linux-Specific Issues

### Missing Dependencies

**Debian/Ubuntu:**
```bash
sudo apt-get install -f
```

**Fedora/RHEL:**
```bash
sudo dnf install missing-package
```

### AppImage Won't Run

**Make it executable:**
```bash
chmod +x SocketZero-*.AppImage
./SocketZero-*.AppImage
```

## Getting More Help

If none of these solutions work:

1. **Collect logs:**
   - Enable debug mode: `SOCKETZERO_DEBUG=1`
   - Reproduce the issue
   - Locate logs (see paths above)

2. **Open an issue:**
   - [GitHub Issues](https://github.com/radiusmethod/socketzero-docs/issues)
   - Include:
     - OS and version
     - SocketZero version
     - Steps to reproduce
     - Relevant log snippets (redact sensitive info)

3. **Contact support:**
   - Email: support@radiusmethod.com
   - Include the same info as above
