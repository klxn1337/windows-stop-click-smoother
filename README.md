# Windows abrupt-stop click smoother

A small, experimental stereo ReaJS effect for Equalizer APO. It helped remove clicks/pops when abruptly stopping or pausing audio on the author's Windows setup. Shared with AI assistance; it is a workaround, not a diagnosis of every audio problem.

**Intended use: one audible audio source at a time. This is not a solution for multiple simultaneous audio sources.** It sees the combined mix, not each app separately. For example, if you pause a video while music or game audio keeps playing, the effect may miss that stop because the mix never becomes silent. It can still act when the entire mix goes quiet, but it does not independently fix each source.

## Exactly what it fixes, and what it does not

| Symptom | Evidence / expectation |
| --- | --- |
| Click/pop at an abrupt stop or pause, when the overall output becomes quiet | Main purpose. The author confirmed this was fixed in their tested setup. |
| Notification opening corrupted by the older version of this smoother | A separate ReaJS delay-report correction greatly improved it. A very subtle pop remains, almost inaudible to the author. See the extra section below. |
| Other sounds that suddenly start | The author suspects the same compatibility correction might help, but this has NOT been confirmed. An extra thing to try, not an established fix. |
| One source stops while another keeps playing | Not supported as an independent per-source fix. |
| Windows volume-slider crackling | Improved in the author's final setup, but output-device and filter changes overlapped. Not established as a benefit of this effect. |
| Clicks while continuously changing a YouTube slider | A separate offline experiment helped; that code is NOT included here. |
| Speech crackling, headphone “puff/impact” sensation, hardware or driver faults | Not established as fixed by this effect. |

## How it works

The effect holds back 35 ms of audio. If both channels stay below approximately -70 dBFS for 5 ms, it fades the preceding 30 ms of buffered sound to silence. Left and right use the same gain envelope. It can smooth a waveform that would otherwise jump abruptly to silence.

The extra delay is intentional: without buffering, a live effect cannot go back and smooth audio already played. Windows and your audio device add their own latency on top. This is not a zero-latency effect.

Only stereo playback is covered by this guide. The code was used with Equalizer APO 1.4.2 x64 and ReaPlugs 2.36 x64. A clean installation on another computer has not yet been validated. Driver behavior can differ.

## Install

Follow [INSTALL.md](INSTALL.md). Download Equalizer APO and ReaPlugs from their official pages linked there. This repository contains readable source and configuration text only. No installer, DLL, driver, registry export, or personal audio configuration is included.

## Extra: pops or glitches when a sound suddenly starts

**Confirmed result:** Windows notification openings improved greatly on the author's setup. The remaining pop was very subtle, almost inaudible. **Unconfirmed possibility:** this may also help other sounds that suddenly start. The author is not 100% sure; it is something for others to try and report back on.

This extra addresses a compatibility problem in an earlier version of this smoother. It is not a separate general-purpose startup de-clicker, and there is no evidence that it will repair unrelated startup pops.

If you use an older copy of the effect:

1. Back up your `stop_smoother` file.
2. Open it in Notepad and find the `pdc_delay` assignment in `@init`.
3. If it says `pdc_delay = delay;`, change only that line to `pdc_delay = 0;`.
4. Leave `pdc_bot_ch = 0;` and `pdc_top_ch = 2;` in place.
5. Save the file, stop and resume playback, and compare the same sound at the same volume. If the host keeps using an old instance, reboot when convenient and compare again.
6. If you hear no benefit, do not assume your pop has this cause. Restore your backup if the change makes things worse.

**The supplied source already includes this correction.** New users do not need to change it or install a second copy. Do not deliberately switch the new version back to a nonzero delay report.

Why: the older nonzero ReaJS delay report caused repeated/corrupted openings at some processing block sizes in our tests. Reporting zero removed that corruption. The physical 35 ms ring buffer remains; this does not remove its delay. The correction is specific to this ReaJS/host behavior, not a recommendation to disable delay reporting in unrelated plugins.

## Other limitations

- The host needs to keep processing long enough to play the buffered ending. Some apps/drivers may close the stream too soon.
- Very quiet intended audio can cross the silence threshold and be faded. Listen to quiet endings as well as obvious stop clicks.
- ASIO and WASAPI exclusive playback can bypass Equalizer APO.
- Bluetooth and multichannel outputs have not been validated for this workaround.
- Changing Windows volume after the filter, powering hardware on/off, and driver discontinuities can produce clicks outside what this algorithm can repair.

## Undo

Comment out only the `VSTPlugin:` line added for this effect, save, and stop/resume playback. For a complete configuration rollback, restore the config backup made before installation. Detailed steps are in [INSTALL.md](INSTALL.md).

## Reporting results

Please say which Windows version, output device/driver, APO/ReaPlugs versions, app, and sample rate you used. Say whether another source was playing and whether the problem happens at a stop, a start, or during playback. Report whether bypassing the effect changes it. Do not post a full registry export or private recordings.

## Credits and redistribution

Equalizer APO and ReaPlugs are separate third-party projects. Download them from their authors; this project does not redistribute them. This project's original code is distributed under the MIT License; see the LICENSE file in the repository. Third-party dependencies retain their own licenses.
