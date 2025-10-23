# adb Screen Recording with FFplay

This repository contains a minimal workflow for mirroring an Android device on a Linux desktop. The included `mirror.sh` script streams video frames from `adb shell screenrecord` directly into `ffplay`, providing a near real-time preview window without storing intermediate files on disk.

## Prerequisites

Before running the script, install the following packages on your Linux machine:

- `android-tools-adb` (provides the `adb` command line tools)
- `ffmpeg` (supplies `ffplay`)

On Debian/Ubuntu based systems you can install them with:

```bash
sudo apt-get update
sudo apt-get install android-tools-adb ffmpeg
```

## Getting Started

Clone the repository and make the script executable:

```bash
git clone https://github.com/ifur404/adbscreenrecord-ffplay.git
cd adbscreenrecord-ffplay
chmod +x mirror.sh
```

Connect your Android device to the computer via USB, ensure USB debugging is enabled, and verify that `adb devices` lists the device.

## Usage

Run the mirroring script from the project directory:

```bash
./mirror.sh
```

The script performs the following steps:

1. Starts a persistent `screenrecord` session on the device with:
   - Bit rate: `16 Mbps`
   - Output format: raw `h264`
   - Resolution: `800x600`
2. Pipes the raw stream to `ffplay`, which displays the feed using:
   - Frame rate cap: `60 fps`
   - Frame dropping enabled to keep latency low
   - Input buffer size: `16 MB`

Press `q` in the `ffplay` window or `Ctrl+C` in the terminal to stop streaming.

## Customisation

You can tweak the streaming parameters by editing `mirror.sh`:

- `--bit-rate` controls video quality and bandwidth usage.
- `--size` sets the mirrored resolution. Larger values increase bandwidth requirements.
- `-framerate` and `-bufsize` on the `ffplay` side let you balance latency versus smooth playback.

## Troubleshooting

- **Device not detected**: run `adb devices` and ensure USB debugging is enabled. Restarting the adb server with `adb kill-server && adb start-server` can help.
- **Laggy playback**: reduce the resolution or bit rate in the script, or ensure the USB connection is using a high-quality cable.
- **Permission errors**: if `adb` requires elevated privileges, prepend the script command with `sudo` or configure `udev` rules for your device.

## License

This project follows the licensing terms stated in the original repository. If no explicit license is present, treat the contents as provided "as is" and attribute the original author (`Ahmad Saepur Ramdan`) when redistributing modifications.
