# Flash and recover the Orin NX

1. Put in recovery mode: hold REC, press RESET, release REC.
2. Check: `lsusb | grep -i nvidia` should show `APX`.
3. Flash: `sudo ./flash.sh jetson-orin-nano-devkit internal`
4. If it hangs at "waiting for target": try a different USB cable/port (USB 2 hub fails often).

Last verified: 2026-09-20, JetPack 6.1
