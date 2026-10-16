# J.A.R.V.I.S DESK Setup Guide

This guide explains how to install and run J.A.R.V.I.S DESK on a Windows computer. It is written for first-time Python users and uses the setup process included with the project.

> **Project credit:** J.A.R.V.I.S DESK — Developed & Customized by Hassaan-dev. Based on Mark-LV by FatihMakes.

## Introduction

J.A.R.V.I.S DESK is a desktop AI assistant with a PyQt6 interface. It uses Google's Gemini Live API for conversation and voice responses, accepts microphone input, can display an animated HUD, and includes tools for browser, file, application, and desktop control.

To run it, you need:

- A supported Python installation
- A microphone and speakers or headphones for voice interaction
- An internet connection
- A Gemini API key
- The project files downloaded from GitHub

The application starts with:

```powershell
python main.py
```

## System Requirements

### Supported operating systems

The project contains platform-specific support for:

- Windows 10 or Windows 11 — primary platform for this guide
- macOS
- Linux

The dependency file automatically skips packages that do not apply to the current operating system. Some desktop-control features are platform-specific, so this guide focuses on Windows.

### Python

- Minimum version: Python 3.11
- Tested versions declared by the project: Python 3.11, 3.12, and 3.13
- Recommended for a new installation: Python 3.13

Python versions older than 3.11 are rejected by `setup.py`. A newer Python version may continue with a warning, but some required packages might not yet provide compatible builds.

### Hardware

- Microphone — required for voice interaction and the optional “Hey Jarvis” wake word
- Speakers or headphones — required to hear voice responses
- GPU — not required; the HUD and avatar render in software
- Webcam — optional and only needed for camera-based features

### Internet access

Internet access is required to:

- Install Python packages
- Download Playwright browser engines during setup
- Connect to the Gemini API
- Use web search, weather, YouTube, and other online features
- Download the optional local wake-word package and model

### Required API service

J.A.R.V.I.S DESK requires a Google Gemini API key. The key is entered inside the application on first launch; no `.env` file is used.

Create or manage a key at [Google AI Studio](https://aistudio.google.com/app/apikey). Availability and usage limits are controlled by Google.

## Installation on Windows

### 1. Install Python

1. Open the [Python downloads page for Windows](https://www.python.org/downloads/windows/).
2. Download a supported 64-bit Python release, preferably Python 3.13.
3. Run the installer.
4. Enable **Add python.exe to PATH** before selecting **Install Now**.
5. After installation, open PowerShell and check the version:

```powershell
python --version
```

The result should report Python 3.11, 3.12, or 3.13.

If `python` is not recognized but the Python launcher is installed, try:

```powershell
py --version
```

### 2. Install Git if needed

Git is only required when cloning the repository. If you download the project as a ZIP file, you can skip this step.

1. Download [Git for Windows](https://git-scm.com/download/win).
2. Install it using the default options.
3. Open a new PowerShell window and verify it:

```powershell
git --version
```

### 3. Download the repository

Choose one method.

#### Option A: Download a ZIP

1. Open the J.A.R.V.I.S DESK repository on GitHub.
2. Select **Code**, then **Download ZIP**.
3. Extract the downloaded ZIP to a permanent folder, such as:

```text
C:\Users\YourName\Documents\JARVIS-DESK
```

Do not run the application from inside the ZIP archive.

#### Option B: Clone with Git

On the repository's GitHub page, select **Code** and copy its HTTPS address. In PowerShell, run:

```powershell
git clone <repository-url>
```

Replace `<repository-url>` with the address copied from GitHub.

### 4. Open the project folder

In File Explorer, open the extracted or cloned folder. Click the address bar, type `powershell`, and press Enter.

Alternatively, change folders from an existing PowerShell window:

```powershell
cd "C:\path\to\JARVIS-DESK"
```

Confirm that the folder contains `main.py`, `setup.py`, and `requirements.txt`:

```powershell
Get-ChildItem main.py, setup.py, requirements.txt
```

### 5. Create a virtual environment

A virtual environment keeps this application's packages separate from other Python projects.

```powershell
python -m venv .venv
```

If `python` does not work, use the Python launcher:

```powershell
py -3.13 -m venv .venv
```

### 6. Activate the virtual environment

```powershell
.\.venv\Scripts\Activate.ps1
```

When activation succeeds, `(.venv)` appears at the beginning of the PowerShell prompt.

If PowerShell says that script execution is disabled, allow locally created scripts for your Windows account:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Close and reopen PowerShell, return to the project folder, and activate the environment again. You can also avoid changing the execution policy by using Command Prompt and running `.venv\Scripts\activate.bat`.

### 7. Update pip

```powershell
python -m pip install --upgrade pip
```

### 8. Install the project dependencies

Run the included OS-aware setup script:

```powershell
python setup.py
```

This script:

- Confirms that the Python version is supported
- Installs packages from `requirements.txt`
- Installs Windows-only packages only on Windows
- Downloads Playwright's Chromium and Firefox browser engines
- Checks that the bundled avatar model is present
- Leaves the optional wake-word package uninstalled until the user enables it

The Playwright download may take several minutes. If that download fails, the rest of the assistant can still work, but browser automation may be unavailable. Retry it with:

```powershell
python -m playwright install chromium firefox
```

For a manual dependency installation, use:

```powershell
python -m pip install -r requirements.txt
python -m playwright install chromium firefox
```

Using `python -m pip` ensures packages are installed into the active virtual environment.

## First Launch and Gemini Configuration

### 1. Start the application

With the virtual environment active and PowerShell open in the project folder, run:

```powershell
python main.py
```

### 2. Complete the initialization screen

On first launch, J.A.R.V.I.S DESK displays an **Initialization Required** screen.

1. Paste your Gemini API key into the **Gemini API Key** field.
2. Confirm that **Windows** is selected. The application should detect it automatically.
3. Select **Initialize Systems**.

The application stores this information locally in:

```text
config\api_keys.json
```

This file is intentionally excluded by `.gitignore`. It stores the API key as plaintext, so do not publish it, attach it to bug reports, or commit it to Git. If a key is accidentally exposed, revoke it in Google AI Studio and create a replacement.

If the key is invalid, the application displays the setup screen again so that a new key can be entered.

### 3. Allow Windows permissions

Windows may request access when a feature is used for the first time:

- Allow microphone access for voice input.
- Allow camera access only if you intend to use camera features.
- Review any Windows Firewall prompt for the optional phone dashboard before allowing access. Private networks are safer than public networks.

Windows microphone permissions are available under **Settings → Privacy & security → Microphone**. Make sure microphone access and desktop-app access are enabled.

## Basic Configuration

Open the **Setup** panel using the gear button in the application header.

### Select audio devices

1. Select **Audio Devices**.
2. Choose the microphone J.A.R.V.I.S DESK should hear.
3. Choose the speakers or headphones it should use.
4. Apply the selection. The live session reconnects while retaining the conversation.

If no device is selected, the application uses the operating system's current default device.

### Customize the assistant

Select **Customize Assistant** to configure the assistant name, user name, interface color, and Gemini voice. The available voices are Charon, Puck, Kore, Fenrir, and Aoede.

### Enable the optional wake word

The local “Hey Jarvis” detector is deliberately not installed during initial setup.

1. Open the **Controls** panel using the controls button in the header.
2. Select **Wake Word: Download**.
3. Wait while `openwakeword` and its model files are downloaded.
4. Enable **Wake Word** when the download completes.

Wake-word detection runs locally. Its first installation requires internet access, but normal detection is offline.

### Push-to-talk

Push-to-talk can be enabled from the Controls panel. On Windows, the default chord is available globally; the microphone remains closed until the configured keys are held.

### Create a desktop shortcut

Open the Setup panel and select **Create Desktop Shortcut**. The shortcut is named **J.A.R.V.I.S DESK** and launches `main.py` from the correct project directory.

Keep the project folder in its original location after creating the shortcut. If you move the folder, create the shortcut again.

## Starting J.A.R.V.I.S DESK Later

Each time you open a new PowerShell window:

```powershell
cd "C:\path\to\JARVIS-DESK"
.\.venv\Scripts\Activate.ps1
python main.py
```

Or use the desktop shortcut created from the application's Setup panel.

To leave the virtual environment after closing the application:

```powershell
deactivate
```

## Updating an Existing Installation

Before updating, close J.A.R.V.I.S DESK. If the project was cloned with Git:

```powershell
cd "C:\path\to\JARVIS-DESK"
git pull
.\.venv\Scripts\Activate.ps1
python setup.py
python main.py
```

Do not delete `config/api_keys.json` or `memory/long_term.json` unless you intentionally want to remove the saved configuration or memory. Both files are local and excluded from Git.

## Troubleshooting

### `python` is not recognized

- Close and reopen PowerShell after installing Python.
- Try `py --version` and use `py` in place of `python` when creating the virtual environment.
- Reinstall Python and enable **Add python.exe to PATH**.

### PowerShell cannot run `Activate.ps1`

Run:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Then reopen PowerShell. Alternatively, activate the environment from Command Prompt with `.venv\Scripts\activate.bat`.

### `ModuleNotFoundError`

Confirm that `(.venv)` appears in the prompt, then reinstall the declared dependencies:

```powershell
python -m pip install -r requirements.txt
```

If the missing package is `openwakeword`, use the in-app **Wake Word: Download** control; it is optional and intentionally omitted from the default installation.

### Microphone is not working

1. Check Windows microphone privacy permissions.
2. Make sure another application is not using the microphone exclusively.
3. Open **Setup → Audio Devices** and select the intended input device.
4. Confirm that the main microphone button does not say **Microphone Muted**.
5. Check the Windows input level under **Settings → System → Sound**.

### No sound from the assistant

1. Check the Windows output volume and selected playback device.
2. Open **Setup → Audio Devices** and select the intended speakers or headphones.
3. Confirm that the application and Windows are not muted.

### Gemini API key is rejected

- Copy the key again from Google AI Studio without spaces or quotation marks.
- Confirm that the computer is online.
- If the key was exposed publicly, revoke it and create a replacement.
- Relaunch the app; an invalid-key response automatically reopens the configuration screen.

### Browser automation is unavailable

Install or repair the browser engines inside the active virtual environment:

```powershell
python -m playwright install chromium firefox
```

### Desktop shortcut creation reports a pywin32 problem

The application includes a Windows fallback, but the primary shortcut method uses pywin32. Repair it with:

```powershell
python -m pip install --force-reinstall pywin32
python ".\.venv\Scripts\pywin32_postinstall.py" -install
```

### The avatar falls back to a glowing core

Confirm that this bundled file exists and is larger than 4 KB:

```text
core\face_model.obj
```

If it is missing or truncated, download the repository again or restore that file from the repository.

### Installation fails while downloading packages

- Confirm that the internet connection is working.
- Disable a VPN or proxy temporarily if it blocks Python package downloads.
- Retry `python setup.py` from the activated virtual environment.
- Read the first error in the PowerShell output; later errors may only be consequences of the first one.

## Security and Privacy Notes

- Never commit `config/api_keys.json`, `.env` files, OAuth tokens, client secrets, or credentials.
- Never share the generated dashboard certificate folder under `config\certs`.
- The Gemini API key is stored locally as plaintext.
- Voice audio is sent to Google's Gemini Live API while a live session is connected.
- When wake-word mode is asleep, microphone frames are processed by the local wake-word detector rather than streamed to Gemini.
- File, application, browser, and desktop-control features can change data or computer settings. Review confirmation prompts carefully.
- Keep `.gitignore` in the repository; it already excludes secrets, saved memory, virtual environments, and generated runtime files.

## Installation Checklist

- [ ] Python 3.11, 3.12, or 3.13 is installed
- [ ] The repository has been downloaded and extracted or cloned
- [ ] `.venv` has been created and activated
- [ ] `python setup.py` completed
- [ ] A Gemini API key is available
- [ ] `python main.py` opens J.A.R.V.I.S DESK
- [ ] Windows microphone permission is enabled
- [ ] The correct microphone and speakers are selected
- [ ] Optional wake-word files have been downloaded if desired

## Getting Help

When reporting a problem, include:

- Windows version
- Python version from `python --version`
- The exact command that failed
- The complete error message or a screenshot with secrets hidden
- Whether the virtual environment was active

Do **not** include your Gemini API key, `config/api_keys.json`, OAuth tokens, credentials, or private dashboard files.

## Credits and License

J.A.R.V.I.S DESK — Developed & Customized by Hassaan-dev. Based on Mark-LV by FatihMakes.

See [LICENSE](LICENSE) for the project's license terms. Required original notices and attribution must be preserved in copies and distributions.
