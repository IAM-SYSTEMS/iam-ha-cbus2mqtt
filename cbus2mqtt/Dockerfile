ARG BUILD_ARCH=amd64
# Pin upstream 0.6.0 (includes micolous/cbus PR #31). Do not float :latest.
FROM mtsamsonite/cbus2mqtt-${BUILD_ARCH}:0.6.0

# Confirm-and-wait send queue. Replaces libcbus PCIProtocol._send firehose.
# Source is this addon's pciprotocol.py (patched 0.6.0 / PR #31 file).
COPY pciprotocol.py /usr/lib/python3.10/site-packages/cbus/protocol/pciprotocol.py
