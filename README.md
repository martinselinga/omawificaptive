# OmaWifiCaptive

A Wi-Fi bar widget for Omarchy, built on top of the stock network widget,
with two things added:

- **Captive portal detection.** Land on a hotel or airport network that
  needs a login page, and the widget badges the icon and gives you a direct
  click to force the sign-in redirect, instead of you discovering the
  internet is broken five minutes later.
- **An encryption traffic light.** The Wi-Fi icon is colored by how the
  current connection is secured — green for WPA3, amber for WPA2, red for
  anything weaker or open. One glance tells you whether you should think
  twice before doing anything sensitive on it.

Everything else — the network list, signal bars, DNS switching, Wi-Fi band
selection, speed test, QR sharing — behaves the same as Omarchy's built-in
network widget, since that's what this started from.

## Install

Review the repository, then add the plugin:

```bash
omarchy plugin add https://github.com/martinselinga/omawificaptive.git
```

Accept the prompt to enable the plugin during installation. Enabling it adds
it to the bar as a `bar-widget`; it does not replace Omarchy's built-in
network widget automatically, so disable or remove that one if you don't
want both.

For an unattended install from a repository you already trust:

```bash
omarchy plugin add https://github.com/martinselinga/omawificaptive.git --enable --yes
```

## Update

```bash
omarchy plugin update martinsel.omawificaptive
```

Or update all Git-managed plugins:

```bash
omarchy plugin update --all
```

## Validate from source

```bash
omarchy plugin validate .
```

## Security

This plugin runs unsandboxed inside `omarchy-shell` when enabled. It shells
out to:

- `omarchy-network-status --verbose`, `omarchy-network-band`, `omarchy-dns`
  — the same status/configuration commands the stock network widget uses.
- NetworkManager directly through Quickshell's `Networking` backend for
  connecting, disconnecting, forgetting networks, and toggling Wi-Fi.
- `nmcli` (via a bundled connect script) when setting up an 802.1X
  enterprise connection; the password is piped over stdin, never passed as
  an argument.
- `wl-copy`, to copy the IP address or gateway to your clipboard when you
  click those fields.
- Your configured browser, to open `http://neverssl.com` when you click the
  portal badge or the "sign in" button — this is the only outbound network
  request the widget itself initiates beyond normal network status
  polling.

No background services beyond the normal polling the bar widget does while
open.

## License

MIT — see [LICENSE](LICENSE).
