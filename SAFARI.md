# Safari Extension Development Guide

## Building for Safari

To build the extension specifically for Safari:

```bash
pnpm build:safari
```

## Development for Safari

For Safari development (without hot-reload to avoid WebSocket issues):

```bash
pnpm dev:safari
```

## Safari Installation & Testing

### 1. Load Extension in Safari

1. Build the extension: `pnpm build:safari`
2. Open Safari and go to Safari → Preferences → Advanced
3. Check "Show Develop menu in menu bar"
4. Go to Develop → Allow Unsigned Extensions (for development)
5. Go to Safari → Preferences → Extensions
6. Click the "+" button and select the `build/safari-mv3-prod` folder

### 2. Grant Permissions

Safari requires manual permission setup:

1. **Enable Extension**: Safari → Preferences → Extensions → Enable "P-Stream extension"
2. **Website Access**: Safari → Preferences → Websites → P-Stream extension → Set to "Allow on all websites"
3. **Reload Pages**: Refresh any streaming website where you want to use the extension

### 3. Common Safari Issues & Fixes

#### Issue: "Invalid call to runtime.connect()"
**Fix**: This occurs when the background script isn't ready. The extension now includes:
- Delayed messaging setup for Safari
- Retry logic for failed connections
- Better error handling

#### Issue: WebSocket Connection Blocked
**Fix**: Use `pnpm dev:safari` which disables hot-reload WebSockets that Safari blocks.

#### Issue: Permissions Not Working
**Fix**: 
- Ensure both `host_permissions` and `optional_host_permissions` are in manifest
- Guide users through Safari's manual permission setup
- Check permissions through Safari Preferences, not runtime API

### 4. Safari-Specific Behavior

- **No Runtime Permission Requests**: Safari doesn't support `chrome.permissions.request()` - users must enable manually
- **Different Timing**: Safari background scripts load differently - we added delays and retry logic
- **Security Policies**: Safari blocks insecure content more aggressively than Chrome
- **Manifest Differences**: Safari requires `host_permissions` for proper website access

### 5. Testing Checklist

- [ ] Extension loads in Safari Extensions preferences
- [ ] Website permissions are granted in Safari Preferences → Websites
- [ ] No "Invalid call to runtime.connect()" errors
- [ ] No WebSocket connection warnings
- [ ] Permission guide shows correctly when Safari is detected
- [ ] All message handlers work properly

### 6. Production Deployment

For Safari App Store distribution:
1. You'll need to create a native macOS app wrapper
2. Use Xcode to create the Safari extension project
3. Follow Apple's Safari extension guidelines
4. The built extension in `build/safari-mv3-prod` can be embedded in the macOS app
