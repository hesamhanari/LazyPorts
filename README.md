# LazyPorts

> **Visual port manager for Linux**

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Go](https://img.shields.io/badge/Go-1.24+-00ADD8.svg)
[![GitHub stars](https://img.shields.io/github/stars/v9mirza/lazyports?style=flat-square)](https://github.com/v9mirza/lazyports/stargazers)
<br>
`lazyports` is a terminal UI tool to visualize and manage network ports. It provides an interactive table to inspect listening processes and kill them easily.

![LazyPorts Demo](assets/image.png)

Built with Bubble Tea and Lipgloss.

> **Fork note:** this is a fork of [v9mirza/lazyports](https://github.com/v9mirza/lazyports). All credit for LazyPorts goes to upstream; this fork's only difference is that it publishes prebuilt Linux binaries on its [Releases page](https://github.com/hesamhanari/lazyports/releases).

## Prebuilt binaries (no Go needed)

Prebuilt static Linux binaries (`amd64` and `arm64`) are published on this fork's [Releases page](https://github.com/hesamhanari/lazyports/releases), together with a `SHA256SUMS` file. No Go toolchain is required.

```bash
# 1. Pick the binary for your CPU (x86_64 -> amd64, aarch64 -> arm64)
case "$(uname -m)" in
  x86_64)        ARCH=amd64 ;;
  aarch64|arm64) ARCH=arm64 ;;
  *) echo "unsupported architecture: $(uname -m)" >&2; exit 1 ;;
esac

# 2. Download the binary and the checksum file
BASE=https://github.com/hesamhanari/lazyports/releases/latest/download
curl -fsSLO "$BASE/lazyports-linux-$ARCH"
curl -fsSLO "$BASE/SHA256SUMS"

# 3. Verify the download (must print "lazyports-linux-...: OK")
sha256sum -c --ignore-missing SHA256SUMS

# 4. Make it executable and install it to /usr/local/bin
chmod +x "lazyports-linux-$ARCH"
sudo mv "lazyports-linux-$ARCH" /usr/local/bin/lazyports

# 5. Run it (needs an interactive terminal)
lazyports
```

Do not continue if step 3 prints `FAILED` or an error. To install a specific version instead of the latest, replace `latest/download` in `BASE` with `download/<tag>` (for example `download/v0.1.0`).

The [Installation](#installation) section below (Go required) remains available as an alternative.

## Prerequisites

-   **Go 1.24** or higher (required for installation)

## Installation

Install with a single command:

```bash
curl -sL https://raw.githubusercontent.com/v9mirza/lazyports/main/install.sh | bash
```

*Note: The script may request `sudo` permission to install the binary to `/usr/local/bin` for system-wide access.*

## Uninstallation

To remove `lazyports` from your system, simply delete the binary:

```bash
# Remove system-wide installation
sudo rm /usr/local/bin/lazyports

# Remove user-local installation
rm ~/go/bin/lazyports
```

## Usage

Run the tool from your terminal:

```bash
lazyports
```

### Controls

| Key | Action |
| :--- | :--- |
| `↑` / `↓` | Navigate list |
| `/` | **Filter / Search** ports |
| `Enter` | **View Details** (User, Path, Time) |
| `k` | Kill selected process |
| `r` | Refresh list |
| `s` | **Sort** (Cycle: Port → Process → PID) |
| `q` | Quit |

## Features

-   **Interactive Table**: Clean visualization of open ports (TCP/UDP).
-   **Sortable Columns**: Press `s` to cycle sorting by Port, Process Name, or PID.
-   **Smart Filtering**: Type `/` to instantly filter by port, PID, or process name.
-   **Detailed Inspection**: Press `Enter` to see full command, user, and start time.

    ![Details View](assets/details.png)

-   **Process Management**: Terminate blocking processes instantly.
-   **Root-Aware detection**: Correctly identifies system processes (returning `(system)`) vs required privileges.
-   **Auto-Sorting**: Ports are automatically sorted numerically by default.
-   **Visual States**: Distinct indicators for `LISTEN` and `ESTAB` connections.
-   **Zero Config**: Works out of the box with automatic shell path configuration.


## License

MIT
