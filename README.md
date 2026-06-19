# YouTube Downloader (Desktop)

A standalone desktop application for downloading YouTube videos, audio, and
playlists. No account, no website, no command line — just run the app, paste a
URL, pick a quality, and download. Files are saved to your **Downloads/YouTube
Downloads** folder.

It is built on [yt-dlp](https://github.com/yt-dlp/yt-dlp) with a small Flask +
HTML/JS UI rendered inside a native window via
[pywebview](https://pywebview.flowrl.com/). `ffmpeg` is bundled, so there is
nothing else to install.

## Why a desktop app (and not a website)?

Running on your own machine uses your home/residential IP. Public, server-hosted
YouTube downloaders are routinely blocked by YouTube with *"Sign in to confirm
you're not a bot"* because they run from datacenter IP ranges. A desktop app
avoids that, needs no hosting, and writes files straight to your Downloads
folder.

## Using a prebuilt release

1. Download `YouTube-Downloader.exe` (Windows).
2. Double-click it. The first launch may take a few seconds while it starts.
3. Paste a YouTube video or playlist URL, click **Fetch Info**, choose
   Video+Audio or Audio Only, pick a quality, and click **Download**.

### Windows requirement: WebView2 Runtime

The app renders its UI with the Microsoft Edge **WebView2** runtime, which is
preinstalled on Windows 11 and most up-to-date Windows 10 machines. If the
window appears unstyled or buttons do nothing, install the Evergreen runtime
(free, from Microsoft):
https://developer.microsoft.com/microsoft-edge/webview2/

## Running from source

Requires Python 3.10+.

```bash
pip install -r requirements.txt
python desktop.py
```

There is also a pure command-line version with no GUI:

```bash
python download.py
```

## Building the standalone executable

From the project root, on the OS you want to build for (Windows builds a
Windows `.exe`, macOS builds a macOS app, etc.):

```bash
pip install -r requirements.txt
pyinstaller --noconfirm --clean YouTube-Downloader.spec
```

The result is a single file in `dist/` (`dist/YouTube-Downloader.exe` on
Windows). It bundles Python, yt-dlp, and ffmpeg, so it runs on machines without
Python installed.

> Note: PyInstaller cannot cross-compile. Build the macOS app on macOS and the
> Linux binary on Linux.

## Project layout

| File | Purpose |
| --- | --- |
| `desktop.py` | Desktop entry point: runs Flask locally and opens the native window. |
| `app.py` | Flask backend: fetch-info / download / status endpoints + yt-dlp logic. |
| `download.py` | Standalone interactive CLI version. |
| `templates/`, `static/` | The web UI rendered inside the desktop window. |
| `YouTube-Downloader.spec` | PyInstaller build configuration. |

## Legal

For personal use. Respect YouTube's Terms of Service and copyright law; only
download content you have the right to.
