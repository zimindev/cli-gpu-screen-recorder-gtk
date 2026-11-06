# **🎥 Complete Game Recording Guide for CLI \ Linux (GPU Screen Recorder) 🐧**

Record your games like a pro! This guide covers **recording, instant replays, audio, and quality settings** using `gpu-screen-recorder` and its GTK app. Encoding runs on the GPU, so recording has very little impact on FPS.

---

## **🛠 Installation on Different Distros**

### **Arch Linux/Manjaro 🏗️**

```bash
# CLI
sudo pacman -S gpu-screen-recorder

# GTK app (AUR)
yay -S gpu-screen-recorder-gtk
```

### **Ubuntu/Debian 🐳**

```bash
sudo apt update
sudo apt install flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub com.dec05eba.gpu_screen_recorder
```

### **Fedora/RHEL 🎩**

```bash
sudo dnf install flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub com.dec05eba.gpu_screen_recorder
```

### **OpenSUSE 🦎**

```bash
sudo zypper install flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub com.dec05eba.gpu_screen_recorder
```

Log out and back in if the app does not appear in the menu after installing it.

### **Run from Flatpak**

Run the app:

```bash
flatpak run com.dec05eba.gpu_screen_recorder
```

Run the CLI from Flatpak:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --help
```

---

## **🏁 Basic Usage**

### **GTK App**

```bash
# Open the app
gpu-screen-recorder-gtk
```

Choose the screen, set the frame rate to 60, choose `Default output` for audio, and start recording.

### **Check Your System**

```bash
# Show the GPU, codecs and containers you can use
gpu-screen-recorder --info

# List monitors
gpu-screen-recorder --list-monitors

# List everything you can capture
gpu-screen-recorder --list-capture-options

# List audio devices
gpu-screen-recorder --list-audio-devices
```

### **Record the Screen**

Press `Ctrl+C` to stop. The file is saved when recording stops.

```bash
# Record the whole screen with game audio
gpu-screen-recorder -w screen -f 60 -k h264 -a default_output -o game.mp4

# Record one monitor by name (see --list-monitors)
gpu-screen-recorder -w eDP -f 60 -k h264 -a default_output -o game.mp4

# Record the focused window (-s is required)
gpu-screen-recorder -w focused -s 1920x1080 -f 60 -k h264 -a default_output -o game.mp4

# Record part of the screen
gpu-screen-recorder -w region -region 1280x720+0+0 -f 60 -k h264 -a default_output -o game.mp4
```

### **Audio**

```bash
# Game audio and microphone in one track
gpu-screen-recorder -w screen -f 60 -k h264 -a "default_output|default_input" -o game.mp4

# Game audio and microphone in two separate tracks
gpu-screen-recorder -w screen -f 60 -k h264 -a default_output -a default_input -o game.mp4
```

Separate tracks are easier to edit later.

---

## **🔁 Instant Replay**

Replay mode keeps the last minutes in memory and writes a file only when you ask for it.

```bash
# Keep the last 5 minutes (-r is in seconds, -o is a directory)
gpu-screen-recorder -w screen -f 60 -k h264 -a default_output -c mp4 -r 300 -o ~/Videos
```

Run this from another terminal or a hotkey:

```bash
# Save the replay
killall -SIGUSR1 gpu-screen-recorder
```

---

## **⌨️ Hotkeys**

### **Default Hotkeys in the GTK App**

| Page      | Hotkey  | Action                |
| --------- | ------- | --------------------- |
| Record    | `Alt+1` | Start or stop         |
| Record    | `Alt+2` | Pause or resume       |
| Replay    | `Alt+1` | Start or stop replay  |
| Replay    | `Alt+2` | Save the replay       |
| Streaming | `Alt+1` | Start or stop         |

The hotkeys work only while the app is running. A saved configuration can differ from these defaults, and a hotkey can be empty.

### **Change Hotkeys in the GTK App**

1. Open `gpu-screen-recorder-gtk`.
2. Open the Record, Replay or Streaming page.
3. Click the button next to the action.
4. Press the new key combination.

Press `Backspace` to remove a hotkey or `Esc` to cancel.

On Wayland, hotkeys depend on your desktop. On KDE Plasma they are managed in the system hotkey settings, and some other compositors do not support global hotkeys. X11 needs no extra setup.

### **Hotkeys in i3**

These hotkeys work without the GTK app. Add them to `~/.config/i3/config`, then press `$mod+Shift+c` to reload i3:

```bash
# Start replay mode
bindsym $mod+F9 exec --no-startup-id gpu-screen-recorder -w screen -f 60 -k h264 -a default_output -c mp4 -r 300 -o ~/Videos

# Save the replay
bindsym $mod+F11 exec --no-startup-id killall -SIGUSR1 gpu-screen-recorder

# Stop
bindsym $mod+F12 exec --no-startup-id killall -SIGINT gpu-screen-recorder
```

Check that these keys are free in your config first.

### **Hotkeys in Other Desktops**

Add a custom shortcut in your system settings and use one of the commands from the Quick Command Reference.

---

## **🎮 Quick Command Reference**

| Command                                     | Action                        |
| ------------------------------------------- | ----------------------------- |
| `gpu-screen-recorder-gtk`                   | Open the app                  |
| `gpu-screen-recorder --info`                | Show GPU and codecs           |
| `gpu-screen-recorder --list-audio-devices`  | List audio devices            |
| `gpu-screen-recorder -w screen -o game.mp4` | Record the screen             |
| `Ctrl+C`                                    | Stop and save                 |
| `killall -SIGINT gpu-screen-recorder`       | Stop and save from a hotkey   |
| `killall -SIGUSR1 gpu-screen-recorder`      | Save the replay               |
| `killall -SIGUSR2 gpu-screen-recorder`      | Pause or resume the recording |

---

## **⚡ Advanced Tips**

```bash
# Smaller files with HEVC
gpu-screen-recorder -w screen -f 60 -k hevc -a default_output -o game.mp4

# Higher quality
gpu-screen-recorder -w screen -f 60 -k h264 -q ultra -a default_output -o game.mp4

# MKV keeps the footage readable if the recording is interrupted
gpu-screen-recorder -w screen -f 60 -k h264 -a default_output -c mkv -o game.mkv
```

---

## **🚀 Performance Tips**

* Use `h264` for the best compatibility with editors and players.
* Use `hevc` when disk space matters more than compatibility.
* Match `-f` to your game frame rate.
* Use a smaller capture area if the integrated GPU struggles.
* Run `--info` first to see which codecs your GPU supports.

---

## 📚 **Additional Resources**

* **GPU Screen Recorder**: [https://git.dec05eba.com/gpu-screen-recorder](https://git.dec05eba.com/gpu-screen-recorder)
* **GTK App**: [https://git.dec05eba.com/gpu-screen-recorder-gtk](https://git.dec05eba.com/gpu-screen-recorder-gtk)
* **Flathub**: [https://flathub.org/apps/com.dec05eba.gpu_screen_recorder](https://flathub.org/apps/com.dec05eba.gpu_screen_recorder)

---

## 📄 **License**

This guide is released under the MIT License. GPU Screen Recorder is a separate project licensed under GPL-3.0.
