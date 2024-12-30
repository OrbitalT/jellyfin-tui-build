## Installation

### Prerequisites
1. Install `mpv` and its development libraries:
   ```bash
   sudo apt update
   sudo apt install mpv libmpv-dev
   ```

### Steps
1. Download the latest release binary from the [Releases](https://github.com/OrbitalT/jellyfin-tui-build/releases) page. Example:
   ```bash
   wget https://github.com/OrbitalT/jellyfin-tui-build/releases/download/v1.0.4/jellyfin-tui
   ```

2. Make the binary executable:
   ```bash
   chmod +x jellyfin-tui
   ```

3. Move the binary to a directory in your system's PATH:
   ```bash
   sudo mv jellyfin-tui /usr/local/bin/
   ```

4. Run `jellyfin-tui`:
   ```bash
   jellyfin-tui
   ```

---

## Fix: Missing `libmpv.so.1`
If you encounter the following error:
```
jellyfin-tui: error while loading shared libraries: libmpv.so.1: cannot open shared object file: No such file or directory
```
### Steps to Fix
#### 1. **Check for Installed Library**
Verify if `libmpv.so.2` or another version exists:
```bash
ldconfig -p | grep libmpv
```
You might see something like this:
```
libmpv.so.2 (libc6,x86-64) => /lib/x86_64-linux-gnu/libmpv.so.2
```

#### 2. **Create a Symbolic Link**
If `libmpv.so.1` is not available, you can create a symbolic link to use `libmpv.so.2` as a substitute:
```bash
sudo ln -s /lib/x86_64-linux-gnu/libmpv.so.2 /lib/x86_64-linux-gnu/libmpv.so.1
```

#### 3. **Verify the Link**
Ensure the link is created:
```bash
ls -l /lib/x86_64-linux-gnu/libmpv.so.1
```

#### 4. **Retry Running**
Run the application again:
```bash
jellyfin-tui
```

---

## Key Bindings
| Key              | Alt             | Action                                      |
|------------------|-----------------|---------------------------------------------|
| `space`          |                 | Play / Pause                                |
| `enter`          |                 | Start playing selected                      |
| `up` / `down`    | `k` / `j`       | Navigate up / down                          |
| `tab`            |                 | Cycle between Artist & Track lists          |
| `shift + tab`    |                 | Cycle further to Lyrics & Queue             |
| `a` / `A`        |                 | Skip to next / previous album               |
| `F1`, `F2`       |                 | Switch tabs (F1 - Library, F2 - Search)     |
| `ESC`            |                 | Return to Library tab                       |
| `left` / `right` | `r` / `s`       | Seek +/- 5s                                 |
| `n`              |                 | Next track                                  |
| `N`              |                 | Previous track or restart current           |
| `+`, `-`         |                 | Volume up / down                            |
| `ctrl + e`       | `ctrl + enter`  | Play next                                   |
| `e`              | `shift + enter` | Enqueue (play last)                         |
| `E`              |                 | Clear queue                                 |
| `d`              |                 | Remove from queue                           |
| `x`              |                 | Stop playback                               |
| `t`              |                 | Toggle transcode                            |
| `q`              | `^C`            | Quit                                        |

---

## Configuration
When you run `jellyfin-tui` for the first time, it will ask for your server address, username, and password. These are saved in a configuration file.

### Configuration File
The default configuration file is located at:
```
~/.config/jellyfin-tui/config.yaml
```

### Example Configuration File
```yaml
# must contain protocol and port
server: 'http://localhost:8096'
username: 'jellyfinusername'
password: 'jellyfinpassword'

persist: false # don't restore session on startup
art: false # don't show cover image
auto_color: false # don't grab the primary color from the cover image
primary_color: '#7db757' # hex or color name ('green', 'yellow' etc.)

# options specified here will be passed to mpv - https://mpv.io/manual/master/#options
mpv:
  af: lavfi=[loudnorm=I=-16:TP=-3:LRA=4]
  no-config: true
  log-file: /tmp/mpv.log

transcoding:
  enabled: true
  bitrate: 128
  # container: mp3
```

---

## Supported Terminals
Not all terminals support all features required by `jellyfin-tui`. The following terminals are tested and work well:
- `kitty` (recommended)
- `iTerm2` (recommended)
- `alacritty`
- `konsole`

---

## Credits
**Author:** [dhonus](https://github.com/dhonus)  
**Project Repository:** [Jellyfin TUI](https://github.com/dhonus/jellyfin-tui)

