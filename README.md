# logrotate-wallets

A logrotate config for miners and pool operators running multiple cryptocurrency wallets. Keeps `debug.log` files under control so you can leave logging enabled and still maintain a small disk footprint -- useful when running on VPS servers with limited storage.

No daemon restarts required. Works across any number of wallets and users with a single config file.

## What it does

- Matches `debug.log` in any hidden-directory wallet under any user's home (`/home/*/.*/debug.log`)
- Rotates when a log hits **100 MB**
- Keeps **3** compressed archives per wallet
- Uses `copytruncate` -- truncates the log in place, no wallet restart or signal needed
- Picks up new wallets and users automatically, no config changes required

## Deploy

```bash
sudo cp wallet-debug-logrotate.conf /etc/logrotate.d/wallet-debug
sudo chown root:root /etc/logrotate.d/wallet-debug
sudo chmod 644 /etc/logrotate.d/wallet-debug
```

Logrotate requires the config file to be owned by root and not group-writable -- skipping these steps will cause it to silently ignore the config.

Logrotate runs daily automatically via `/etc/cron.daily/logrotate` or `systemd logrotate.timer`. No cron job needed.

### Test before deploying

```bash
# Dry run -- shows what would happen, no changes made
sudo logrotate -d /etc/logrotate.d/wallet-debug

# Force rotate now
sudo logrotate -f /etc/logrotate.d/wallet-debug
```

## Customization

Open `wallet-debug-logrotate.conf` and adjust these values to suit your available disk space:

| Setting | Default | Description |
|-|-|-|
| `size 100M` | 100 MB | Rotate when log exceeds this size. Use `M` for MB or `G` for GB |
| `rotate 3` | 3 | Number of archived logs to keep per wallet before deleting the oldest |
| `/home/*/.*/debug.log` | all users | Narrow to a specific user with `/home/myuser/.*/debug.log` |

After changing the config, test with a dry run before deploying:

```bash
sudo logrotate -d /etc/logrotate.d/wallet-debug
```

See [wallet-debug-logrotate.md](wallet-debug-logrotate.md) for full details on config options and archived log naming.

## Donate

If this saves your disks, tips are appreciated:

| Coin | Address |
|-|-|
| LTC | `LWpuHQUGw3qZg8MCHYGgTPRZ3a1i8jc5u3` |
| BTC | `bc1qw0t40dunylgtz9kgfylwxac3a8vwp70cgrga5r` |
| SOL | `up9YvW6ewNati5fmmDjGHzFbq8UkSbHPccoDNzijk3G` |
| POL | `0x5198f52fA768294ae66f0cB75A98DCc895a36F2E` |
| DOGE | `DJAg2fTzS5vN3yXDPn5gGPz9JSmzJn3cXD` |
