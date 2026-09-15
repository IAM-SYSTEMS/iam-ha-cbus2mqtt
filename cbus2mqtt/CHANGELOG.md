# Changelog

## v0.6.1-iam
- IAM SYSTEMS fork of MtSamsonite/ha-cbus2mqtt 0.6.0.
- Bakes confirm-and-wait C-Bus TX queue into libcbus `PCIProtocol`.
- One confirmed packet in flight; 350 ms wait; one retry on timeout / `!`.
- Builds locally from `mtsamsonite/cbus2mqtt-{arch}:0.6.0` plus `pciprotocol.py` overlay (no pre-built `image:` pull).
- Fixes 5+ simultaneous HA light commands dropping on 5500CN (`micolous/cbus#22`).

## Upstream v0.6.0
- Added usb and uart access to container access for serial devices.

## Upstream v0.5.0
- Changed containers access to Home Assistant file system from /config to /share.

## Upstream v0.4.0
- First Beta release based on a built image rather than local build.
