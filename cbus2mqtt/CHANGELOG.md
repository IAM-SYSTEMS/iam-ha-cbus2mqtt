# Changelog

## v0.6.2-iam
- Scene ramp to level 0 now publishes MQTT `state: OFF` (brightness 0).
- Stock cmqttd published `state: ON` for every ramp, so a C-Bus scene that turned lights off left Home Assistant showing them on.
- Overlay is `cmqttd.py` copied into the Python 3.10 image, same pattern as `pciprotocol.py`.
- Home Assistant ignores a brightness of 0 and uses the `state` field. A retained `state: ON` / `brightness: 0` message from before this fix still shows as on until it is republished.

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
