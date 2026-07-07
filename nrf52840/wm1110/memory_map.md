# Flash and RAM Memory Map — WM1110 Asset Tracker (nRF52840)

Detailed memory layout from `pm_static_wio_tracker_1110_nrf52840.yml` and build ELF analysis.

**Source file:** `<ncs>/<app>/pm_static_wio_tracker_1110_nrf52840.yml`

**Plain-English summary:** see `context/hardware_quick_ref.md` → Flash Memory Partition Map.

**Chip:** nRF52840 — 1 MB internal flash, 256 KB SRAM.

---

## Flash Map (1 MB total)

| Addr range (hex) | Size | % of 1 MB | Purpose / Notes |
|---|---|---|---|
| 0x00000000 – 0x00000FFF | 4,096 B | 0.39% | **boot_mbr** — Nordic Master Boot Record |
| 0x00001000 – 0x000CFFFF | 847,872 B | 80.86% | **app partition** (max application size) |
| ↳ App USED (from ELF) | 661,311 B | 63.08% | ≈ text + data (actual application content) |
| ↳ App FREE (headroom) | 186,561 B | 17.79% | space left to grow app within app partition |
| 0x000D0000 – 0x000D0FFF | 4,096 B | 0.39% | **mfg_storage** — Sidewalk identity / manufacturing page |
| 0x000D1000 – 0x000D1FFF | 4,096 B | 0.39% | **sidewalk_storage** — Sidewalk runtime state |
| 0x000D2000 – 0x000E1FFF | 65,536 B | 6.25% | **settings_storage** — NVS / PSA ITS (16 × 4 KB sectors) |
| 0x000E2000 – 0x000F2FFF | 69,632 B | 6.64% | **reserved** — future use |
| 0x000F3000 – 0x000F3FFF | 4,096 B | 0.39% | **hw_unique_key_partition** — hardware unique key |
| 0x000F4000 – 0x000FBFFF | 32,768 B | 3.13% | **UF2 bootloader** — Adafruit UF2 drag-drop bootloader |
| 0x000FC000 – 0x000FDFFF | 8,192 B | 0.78% | **bootloader_data** |
| 0x000FE000 – 0x000FEFFF | 4,096 B | 0.39% | **bootloader_mbr_params** |
| 0x000FF000 – 0x000FFFFF | 4,096 B | 0.39% | **bootloader_settings** |

### Flash layout diagram

```
0x00000000  [ boot_mbr 4KB ]
0x00001000  [ app partition ~828 KB — firmware lives here ]
0x000D0000  [ mfg_storage 4KB ]     ← Sidewalk provisioning identity
0x000D1000  [ sidewalk_storage 4KB ]
0x000D2000  [ settings_storage 64KB ] ← NVS (CONFIG_SETTINGS_NVS_SECTOR_COUNT=16)
0x000E2000  [ reserved ~68 KB ]
0x000F3000  [ hw_unique_key 4KB ]
0x000F4000  [ UF2 bootloader 32KB ]  ← WM1110_BOOT drag-drop region
0x000FC000  [ bootloader_data 8KB ]
0x000FE000  [ bootloader_mbr_params 4KB ]
0x000FF000  [ bootloader_settings 4KB ]
0x00100000  (end of 1 MB flash)
```

### Important flash addresses

| Address | Partition | Relevance |
|---|---|---|
| **0x000D0000** | mfg_storage | Provisioning script reports `Address found 0xd0000` — Sidewalk identity UF2 writes here |
| **0x000F4000** | UF2 bootloader | Corrupted if `nrfjprog --chiperase` used without re-flashing bootloader hex |
| **0x000D2000** | settings_storage | NVS — erasing chip loses settings; reprovision if mfg_storage also erased |

**Warning:** `nrfjprog --chiperase` or `west flash --erase` erases **all** nRF52840 flash partitions including UF2 bootloader and Sidewalk identity. LR1110 modem firmware in the module is **not** affected.

---

## RAM Summary (256 KB internal SRAM)

**Device total RAM:** 262,144 B (100%) — SRAM range `0x20000000` – `0x2003FFFF` (`sram_primary` size `0x40000`).

| Category | Bytes | % of RAM | Meaning |
|---|---|---|---|
| Static USED (.data) | 8,467 | 3.23% | Initialized globals (RAM at runtime) |
| Static USED (.bss) | 59,440 | 22.68% | Zero-initialized globals |
| Static USED (noinit) | 61,244 | 23.37% | Reserved, not auto-initialized |
| **Static USED total** | **129,151** | **49.28%** | Fixed RAM at link time |
| **Static FREE (headroom)** | **132,993** | **50.72%** | Before stacks / heap / runtime buffers |

### RAM notes

- **Static headroom (~133 KB)** is available for stack growth, heap (`CONFIG_HEAP_MEM_POOL_SIZE`), and runtime buffers — not all of it is safely usable at peak load.
- BLE + Sidewalk + USB CDC stacks consume runtime RAM beyond static sections — watch for stack overflow if adding large buffers.
- **noinit** section (61 KB) is often used for persistent RAM retention across soft resets — do not assume it is free heap.

---

## Build / Config Cross-References

| Topic | Config / file |
|---|---|
| NVS sector count | `CONFIG_SETTINGS_NVS_SECTOR_COUNT=16` in `prj.conf` (64 KB = 16 × 4 KB) |
| Partition source | `pm_static_wio_tracker_1110_nrf52840.yml` |
| Sysbuild | Mandatory — `sysbuild.conf` manages partition layout |
| App partition DT | `CONFIG_USE_DT_CODE_PARTITION=y` |

---

## When to Check Memory Map

| Question | Where to look |
|---|---|
| Will my code change fit? | App FREE headroom (~186 KB in current build) |
| Provisioning failed / identity missing | mfg_storage @ 0xD0000 |
| Settings not persisting | settings_storage 64 KB NVS |
| WM1110_BOOT missing | UF2 bootloader @ 0xF4000 |
| NVS init errors | Sector count must match 64 KB partition — see `uart_errors.md` |


# Memory Map

[TODO: locate SVD file and pass svd_path]
