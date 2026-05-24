# Release Notes v1.5.15

This release note reflects changes from `v1.5.14` to `v1.5.15`.

## Bug Fix

- **Fixed Galaxy Watch Wear OS 5 pairing failure (sc=1 persisting despite runtime override)**: v1.5.14 set `ble_hs_cfg.sm_sc = 0` at runtime to disable Secure Connections, but NimBLE's internal reset/sync sequence (triggered when the FreeRTOS host task starts) resets `ble_hs_cfg` to its compiled-in defaults — which had `sm_sc=1` because `CONFIG_BT_NIMBLE_SM_SC: "y"` was set in sdkconfig. The runtime override was silently discarded, leaving SC enabled and causing the Galaxy Watch to reject pairing with SMP error 0x07.
  - Changed `CONFIG_BT_NIMBLE_SM_SC` from `"y"` to `"n"` in sdkconfig_options in both YAML files. This disables SC at the compile level so the default in `ble_hs_cfg` is `sm_sc=0`, which NimBLE's reset sequence cannot override.
  - The `ble_hs_cfg.sm_sc = 0` runtime line is retained as belt-and-suspenders.
  - Legacy pairing fully supports IRK (ID key) distribution. Apple and Android devices negotiate legacy pairing correctly when the peripheral advertises SC=0. IRK capture on all previously working devices is unaffected.

## Files Updated

- `components/irk_capture/irk_capture.cpp`
- `ESPHome Devices/irk-capture-base.yaml`
- `ESPHome Devices/irk-capture-full.yaml`
- `RELEASE_NOTES_v1.5.15.md`
