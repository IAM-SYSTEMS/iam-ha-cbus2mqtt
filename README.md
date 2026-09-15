# ha-cbus2mqtt (IAM SYSTEMS)

Public IAM SYSTEMS fork of [MtSamsonite/ha-cbus2mqtt](https://github.com/MtSamsonite/ha-cbus2mqtt) **0.6.0**.
Repo: https://github.com/JoshKelleway/ha-cbus2mqtt-iam

Adds a confirm-and-wait send queue in libcbus so Home Assistant can fire 5+
C-Bus lights in one action without the CNI dropping packets
([micolous/cbus#22](https://github.com/micolous/cbus/issues/22)).

Upstream image is frozen (`mtsamsonite/cbus2mqtt-{arch}:0.6.0`, includes
[PR #31](https://github.com/micolous/cbus/pull/31)). This addon **builds on
the HA host** (no `image:` key) and overlays `cbus2mqtt/pciprotocol.py`.

## Behaviour

- `asyncio.Queue` + `_tx_loop` in `PCIProtocol.connection_made`
- Confirmed packets: one in flight, 350 ms confirm wait, one retry
- Unconfirmed / PCI reset packets still write immediately
- Log line: `cbus tx-queue confirm-and-wait active (1 in flight, 1 retry)`
- MQTT state is still published immediately (optimistic). Fix later if needed.

Do **not** block `_send()` with `threading.Event.wait()` — MQTT callbacks
run on the asyncio loop and that deadlocks.

## Install (HAOS)

1. Add this repository in HA → Settings → Add-ons → Add-on Store → Repositories:
   `https://github.com/JoshKelleway/ha-cbus2mqtt-iam`
2. Install **cbus2mqtt (IAM)** (`cbus2mqtt_iam`). First start builds the image.
3. Copy MQTT/CNI options from the old MtSamsonite addon.
4. Start IAM addon, stop the old `a9f92ca1_cbus2mqtt` addon (only one CNI TCP session).
5. Confirm log marker + 5-light physical test.
6. Remove `/share/cbus/pciprotocol.py` if a previous hotpatch is present, so
   the old entrypoint hook cannot overlay this image.

Same options as upstream 0.6.0 (`cbus_connection_string`, MQTT, etc.).

## Not for Unraid standalone cmqttd

`geoffh1977/cmqttd` (Python 3.8, no PR #31) is a different image. See Odoo
task **gardine-bronte**.

## Layout

```
cbus2mqtt/
  Dockerfile          FROM mtsamsonite/cbus2mqtt-${BUILD_ARCH}:0.6.0 + COPY
  config.yaml         version 0.6.1-iam, no image: key
  pciprotocol.py      patched drop-in
  pciprotocol.py.orig stock 0.6.0 / PR #31
  pciprotocol.py.diff unified diff
```

## License

Upstream addon license plus libcbus LGPL (see `cbus2mqtt/LICENSE`).
