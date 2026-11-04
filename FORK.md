# Fork

This is a fork of [signde/firetv-passthrough](https://github.com/signde/firetv-passthrough) v0.2.1 (commit `1f0d0bb`).

v0.2.1 restores HDMI bypass through a refresh-rate change on Cube 3 / Gazelle by rewriting `hdmi_format=5` inside the audio HAL. Stick 4K Max 2 / Karat does not have that hook. Its Dolby component only reapplied bypass at boot, on wake, and after a framework or audio-server restart. The XDA post for the earlier combined module still lists mode 5 reverting as an open issue.

v0.2.2 adds that Karat case to `scripts/dolby-service.sh`. When `AudioOutputFormatManager` logs `hdmi_format=5`, the module sets bypass again with the same bounded checks used after boot. If the following two seconds also log an HDMI open failure for device 400 and no output is playing, it restarts `vendor.audio-hal` once. A playing stream, or a restart in the past 180 seconds, skips that restart.

DTS-HD hooks, the Gazelle FEL helpers, the firmware profiles, and `bin/frida-inject` are the v0.2.1 files. The Frida 17.22.2 source companion is the upstream asset:

https://github.com/signde/firetv-passthrough/releases/download/v0.2.1/frida-17.22.2-source-and-notices.tar.gz

SHA256 `0871c70158de96aa58e97c4a4dfae7722c8d0deb7a48adef38ff6c4232620b09`.

The refresh-rate path was tested on Karat `RS8185.3891N` with the Dolby-only module `gazelle_ddplus_bypass` 0.3.1: boot restored mode 6, and a mode-5 write during the listener restored mode 6. The HAL-restart latch was not exercised on device. DTS-HD playback with this package has not been tested on `RS8185.3891N`. That build's two audio libraries match `firmware/karat-rs8180.sha256`, which is why the installer accepts it.

Installing this module disables `gazelle_ddplus_bypass`, `firetv_dtshd_passthrough`, and `firetv_video` when those modules are enabled. The module id remains `firetv_passthrough`, so it upgrades an existing Fire TV AV install.
