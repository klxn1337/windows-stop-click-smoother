# Installation — step by step

This guide is for Windows 10/11 on a normal **64-bit Intel/AMD PC**, using one stereo playback output and one audible source at a time. ARM and 32-bit installations are not covered. Allow about 15–30 minutes and one reboot for a new APO installation.

It targets clicks when you stop/pause audio and the whole mix becomes quiet. It does not independently fix several simultaneous audio sources. The optional startup/notification note is at the end.

## 1. Identify the output you actually use

1. Right-click the speaker icon by the clock and open **Sound settings**.
2. Under **Output**, note the selected device name. It might be “Speakers — Realtek Audio”, a USB DAC, or your headphones.
3. Use this same device throughout installation. Do not select your microphone, monitor speakers, or another output by accident.
4. Turn the volume down to a comfortable level before testing. Loud tests are unnecessary.

## 2. Back up any existing APO setup first

1. Press **Windows + E** to open File Explorer.
2. Click the address bar, paste `C:\Program Files\EqualizerAPO`, and press Enter.
3. If the folder exists, copy its entire `config` folder to a dated folder in Documents, such as `Audio backup before stop smoother`.
4. If you already use Peace, another custom APO configuration, or an existing stop smoother, **do not replace config.txt or install a second smoother**. This beginner guide uses a simple fresh APO configuration. Have the addition merged into your existing device/channel/stage structure first. The single plugin line is shown in step 6 for reference.
5. If APO is not installed yet, continue to step 3. After installing it, make the same backup before editing config.txt.

Keep this backup. It preserves your configuration, not every driver or Windows audio setting.

## 3. Install Equalizer APO from its official project

Official download: [Equalizer APO on SourceForge](https://sourceforge.net/projects/equalizerapo/).

Official installation/troubleshooting instructions: [Equalizer APO documentation](https://sourceforge.net/p/equalizerapo/wiki/Documentation/).

1. Download the **x64 / 64-bit** installer from the official project. The tested version was 1.4.2; future versions may look different.
2. Run it and use the default folder: `C:\Program Files\EqualizerAPO`.
3. When the audio-device selection window appears, select **only the playback output you identified in step 1**. Older documentation calls this tool “Configurator”; the tested version has `DeviceSelector.exe`.
4. Leave microphones/capture devices and other outputs unselected. Take a screenshot of the selection for your records.
5. Finish installation and reboot when prompted. Save any open work before rebooting.
6. Do not experiment with registration/troubleshooting modes unless you have a specific reason. If the ordinary installation does not work, use the official troubleshooting guide first.

## 4. Install ReaPlugs from Cockos

Official download: [REAPER / ReaPlugs](https://www.reaper.fm/reaplugs/).

1. Click **ReaPlugs VST v2.36 64-bit** on that page, or the equivalent current 64-bit ReaPlugs link. Download ReaPlugs, not the full REAPER installer.
2. Run the installer. Ensure **ReaJS** is included if you are offered a component list.
3. Choose this destination so the following paths match:
   `C:\Program Files\VSTPlugins\ReaPlugs`
4. Finish. You do not need to buy or run REAPER.
5. In File Explorer, open that destination and find `reajs.dll`.
6. If it is missing, check the actual installation destination. Do not download a loose DLL from a third-party website.

## 5. Create the dedicated smoother folder

The source download for this project contains `reajs_stopsmoother.ini` and `JS\Effects\stop_smoother`. It intentionally does not contain ReaJS itself.

1. On the project's GitHub page, click **Code → Download ZIP**.
2. Right-click the downloaded ZIP and select **Extract All**. Work from the extracted folder, not the ZIP preview.
3. In File Explorer, enable file extensions: on Windows 11 use **View → Show → File name extensions**. On Windows 10 use the **View** tab and check **File name extensions**.
4. Open `C:\Program Files\EqualizerAPO\config` and create a folder named `StopSmoother`. Approve the administrator prompt if Windows shows one.
5. Copy `reajs.dll` from the ReaPlugs installation into this new folder. **Copy it; do not move the original.**
6. Rename the copied DLL to `reajs_stopsmoother.dll`.
7. Copy the supplied `reajs_stopsmoother.ini` into the same folder. Its name must not end with `.ini.txt`.
8. Copy the supplied `JS` folder into `StopSmoother`. Make sure `JS\Data` exists too; if your download omitted the empty folder, create a folder named `Data` inside `JS`.
9. Check that the final layout is exactly:

```text
C:\Program Files\EqualizerAPO\config\StopSmoother\
    reajs_stopsmoother.dll
    reajs_stopsmoother.ini
    JS\
        Data\
        Effects\
            stop_smoother
```

The last file has **no extension**: not `stop_smoother.txt`. Keep the INI beside the DLL. The supplied INI uses `rootpath=JS`, which ReaJS resolves relative to the copied DLL. Cockos documents this behavior in the installed `reajs_info.txt`.

## 6. Load it in a fresh APO configuration

**Only use the replacement below for a fresh/default APO setup with no custom settings.** If you already have EQ, Peace, routing, other plugins, or another smoother, go back to step 2. Do not erase those settings.

1. Back up the newly installed `config` folder if you have not already done so.
2. Open `C:\Program Files\EqualizerAPO\config\config.txt` in Notepad.
3. For a fresh/default setup, replace the bundled sample configuration with:

```text
# Abrupt-stop smoother. Stereo playback only.
# Only install APO on the intended playback output.
Device: all
Channel: L R
VSTPlugin: Library "C:\Program Files\EqualizerAPO\config\StopSmoother\reajs_stopsmoother.dll"
```

4. Save. If access is denied, close Notepad, find Notepad in Start, right-click it and choose **Run as administrator**, then open the file through **File → Open**. Set the file picker to **All files** if needed.
5. Verify the file is still called `config.txt`, not `config.txt.txt`.
6. Here `Device: all` means every output on which you installed APO. That is why step 3 selects only your intended playback output. It does not select a particular brand of headphones. Do not use this broad device line unchanged in a setup with multiple APO-enabled outputs.
7. The main configuration should reload automatically. Stop and resume playback after saving. The initial APO installation still requires its reboot.

## 7. Check that it actually works

1. Close or pause other audible sources: music, games, voice chat, extra browser tabs, and background sound generators.
2. Use the same app/content where you can repeatedly hear a stop or pause click.
3. Start at a comfortable volume, let the sound play, then pause it. Repeat a few times.
4. Also listen to normal music endings and very quiet passages. Check that the effect is not fading something you wanted to hear.
5. Compare with the effect bypassed using step 8. Keep the volume the same for the comparison.
6. Do not treat “it loaded” as proof that it fixed your symptom. Listen and compare.

If saving config.txt appears to do nothing, open Equalizer APO's Configuration Editor (`Editor.exe`) and look for loading errors. Confirm the playback device matches the one chosen during installation and that audio enhancements are not disabled for it. APO requires Windows' effects path; ASIO/WASAPI exclusive paths can bypass it. See the [official troubleshooting guide](https://sourceforge.net/p/equalizerapo/wiki/Documentation/).

If quiet tails are cut off, the host may not be draining the buffer. Earlier USB testing used a device-specific `AllowSilentBufferModification` change, while the current tested Realtek setup leaves it false. **There is no universal registry change included in this beginner guide.** Report your device/driver and symptoms rather than importing someone else's registry export.

## 8. Turn it off or undo it

To bypass just this effect:

1. Open config.txt.
2. Put `#` at the start of the added plugin line:

```text
# VSTPlugin: Library "C:\Program Files\EqualizerAPO\config\StopSmoother\reajs_stopsmoother.dll"
```

3. Save, then stop and resume playback. Remove that leading `#` to turn it on again.
4. If playback does not recover after bypassing, restore the configuration backup and reboot when convenient.

For a full config rollback, restore your own backed-up config folder. If you installed APO only for this experiment and want to remove its device processing too, use its Device Selector/Configurator to deselect the same playback output, then reboot. Do not deselect devices used by other APO effects. Removing ReaPlugs is unnecessary just to bypass the smoother.

## Extra: suddenly starting sounds / Windows notifications

The supplied version already has the ReaJS delay-report correction. **No additional installation or second effect is needed.**

If you have an older copy with `pdc_delay = delay;`, see the [separate extra instructions in README.md](README.md#extra-pops-or-glitches-when-a-sound-suddenly-starts).

Windows notification openings improved greatly in testing; the author describes the remaining pop as very subtle, almost inaudible. They suspect other suddenly starting sounds might benefit too, but do not know for sure. Try and report results; this is not a confirmed universal startup-pop fix.
