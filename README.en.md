[🇻🇳 Tiếng Việt](README.md) · **🇬🇧 English**

# Vietnamese Voice Mic


**Speak Vietnamese into any field on Windows → the text appears right where you need it, no typing.**

A Windows background app for entering Vietnamese by voice into chat boxes and input fields.

> This is the older version. The app has since been renamed **[VoiNoi](https://github.com/ducdg88/voinoi)** (version 2.0.0 and later).

## Main features

- Triggered by `Alt + left click` on the exact field you want to type into.
- Locks the original window and position, then pastes back into that spot after recognition.
- Vietnamese recognition with Google Speech Recognition, with confidence shown in the log/HUD when Google returns it.
- VAD with WebRTC/RMS to know when you are speaking and when you have stopped.
- Optimized for long speech: cuts chunks at low volume regions, sends chunks in parallel, merges and removes duplicates.
- HUD/ring shows the state: listening, recognizing, text ready.
- Paste protection: if the target window is closed, pasting is skipped.
- Recovery: the latest transcript stays on the clipboard and is saved to `voice-last.txt`,
  history is saved to `voice-transcripts.jsonl`, the latest audio to `voice-last.wav`.
- Can build a Windows `.exe`, a release zip and an update manifest.
- Auto-update reads the manifest from the latest GitHub Release.
- Personal data and learned context are stored locally and never pushed to GitHub.

## Quick start

1. Run `Start Vietnamese Voice Mic.cmd`.
2. Hold `Alt` and left-click the chat box or input field you want to type into.
3. When the ring shows `DANG NGHE` (listening), say what you want to type.
4. When you stop speaking, the app switches to `DANG NHAN DIEN` (recognizing).
5. When the result is ready, the app shows a preview and pastes it into the spot you `Alt + click`ed.
6. After recognition you can press `Ctrl+V` to paste the latest transcript again if needed.
7. While listening, press `Esc` to stop and process the audio recorded so far.

## Recover what you just said

If the app cut a sentence, failed to paste, or you switched windows so the target is no longer valid:

- Press `Ctrl+V` to paste the latest transcript again, since the app keeps it on the clipboard.
- Open `voice-last.txt` to see the latest transcript.
- Open `voice-transcripts.jsonl` to see the history of recognitions.
- `voice-last.wav` keeps the latest audio, useful to check what was recorded.

These recovery files live on your machine and are listed in `.gitignore`, never pushed to GitHub.

## Microphone settings

Config file:

```text
voice-mic-settings.json
```

Example:

```json
{
  "preferred_microphone": "BKD-11 Pro Audio",
  "microphone_name_hints": [
    "BKD-11 Pro Audio",
    "USB Audio Device",
    "Microphone",
    "Headset"
  ],
  "enable_particle_effect": true,
  "enable_context_memory": true
}
```

To let the app pick a mic from the hint list, set:

```json
"preferred_microphone": ""
```

## Run from source

Requirements:

- Windows 10/11
- Python 3.10 or later
- A working microphone
- Internet for Google Speech Recognition

Install libraries:

```powershell
python -m venv .venv
.\.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Run the app:

```powershell
python .\voice_mic_icon.py
```

Or:

```text
Start Vietnamese Voice Mic.cmd
```

## Start with Windows

Run PowerShell in the project folder:

```powershell
powershell -ExecutionPolicy Bypass -File .\install-startup.ps1
```

Remove auto-start:

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-startup.ps1
```

## Build a release

Run:

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

Output:

```text
dist\VietnameseVoiceMic\VietnameseVoiceMic.exe
releases\VietnameseVoiceMic-windows.zip
releases\version.json
```

Share this folder with others:

```text
dist\VietnameseVoiceMic
```

Or upload the zip in `releases` to a GitHub Release.

## Push to GitHub

Check changes:

```powershell
git status
git diff --stat
```

Commit:

```powershell
git add .
git commit -m "Improve Vietnamese Voice Mic dictation"
```

Push to GitHub:

```powershell
git push origin main
```

## Shipping a new version

The build script creates `releases\version.json` with:

- `version`
- `zip_url`
- `sha256`
- `notes`
- `release_url`

Auto-update reads the manifest from:

```text
https://github.com/Ducpt88/VietnameseVoiceMic/releases/latest/download/version.json
```

For a new version, run `build.ps1`, then upload these 2 files to the GitHub Release:

```text
releases\VietnameseVoiceMic-windows.zip
releases\version.json
```

## Troubleshooting

- Voice not picked up: check the mic in Windows and `preferred_microphone`.
- Sentences cut too early: increase `WEBRTC_VOICE_END_SECONDS` and `RMS_VOICE_END_SECONDS` in `voice_mic_icon.py`.
- Many wrong words: speak closer to the mic, reduce background noise, or add keywords to `speech_context_terms`.
- Not pasted in the right place: `Alt + click` exactly on the input field before speaking.
- App does not trigger: you must hold `Alt` while left-clicking the input field.

## Key files

- `voice_mic_icon.py`: main app code.
- `voice-mic-settings.json`: mic, context and update settings.
- `voice-mic-settings.local.json`: per machine settings, not pushed to GitHub.
- `voice-context.json`: default public memory, no personal transcripts.
- `voice-context.local.json`: memory learned on each machine, not pushed to GitHub.
- `voice-last.txt`: latest transcript for recovery.
- `voice-transcripts.jsonl`: local transcript history.
- `voice-last.wav`: latest recorded audio on the local machine.
- `Start Vietnamese Voice Mic.cmd`: run the app from source.
- `Start-VietnameseVoiceMic.ps1`: restart the app and make sure only one instance runs.
- `build.ps1`: build the exe, release zip and manifest.
- `updater.ps1`: update helper.
- `install-startup.ps1`: install auto-start with Windows.
- `uninstall-startup.ps1`: remove auto-start.


---

Made by [DUCPT](https://ducpt.com/?utm_source=github&utm_medium=readme&utm_campaign=VietnameseVoiceMic): AI agents, automation and digital products for one-person businesses. This tool: https://ducpt.com/bai-viet/vietnamese-voice-mic-coding-bang-giong-noi/?utm_source=github&utm_medium=readme&utm_campaign=VietnameseVoiceMic
