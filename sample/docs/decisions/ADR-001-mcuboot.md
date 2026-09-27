# ADR-001: Use MCUboot for OTA
Date: 2026-09-25
Context: Need secure OTA on nRF52 and STM32 products with the same process.
Decision: MCUboot, swap-using-move mode, images signed with ECDSA P-256.
Alternatives: Custom bootloader (too much maintenance), vendor DFU (different per chip).
Consequences: +32KB flash for bootloader, need signing key in CI, need 2 image slots.
