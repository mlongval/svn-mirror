# VIC-II Kawari Merge Notes

This directory contains the VIC-II Kawari FPGA extension support merged from
Randy Rossi's vicii-kawari-vice fork onto VICE trunk (as of r46011, 2026-03-15).

## What is VIC-II Kawari?

An FPGA replacement chip for the C64's VIC-II that adds:
- 64k VRAM
- Hires modes: 160x200x16, 320x200x16, 640x200x4
- 80-column mode
- DMA, block copy/fill, math operations
- Programmable RGB/YUV colour palette

See: https://github.com/randyrossi/vicii-kawari

## Emulator Support Status

| Emulator  | Status  | Notes                                      |
|-----------|---------|--------------------------------------------|
| x64sc     | Working | All kawari demos confirmed working         |
| xscpu64   | Borked  | Kawari features not functional in SCPU64   |

The xscpu64 binary also does not accept a `-kernal` option, so JiffyDOS kernal
replacement is not available for that target.

## Files Changed

All changes are confined to `src/viciisc/`:

- `viciitypes.h` — doubled screen/buffer width constants; added hires_* fields to vicii_s struct
- `vicii-color.h` / `vicii-color.c` — kawari colour palette functions
- `vicii-draw.c` / `vicii-draw-cycle.c` — kawari hires rendering
- `vicii-fetch.h` / `vicii-fetch.c` — kawari graphics fetch logic
- `vicii-irq.h` / `vicii-irq.c` — added vicii_irq_dma_set/clear
- `vicii-mem.h` / `vicii-mem.c` — kawari register map, DMA, blit, flash state machines
- `vicii-cycle.c` — kawari cycle handling
- `vicii.c` — kawari init/reset integration

## Upstream Changes Applied

When merging, the following upstream-only fixes were carried forward into the
kawari versions of changed files:

- `log_debug()` API updated to `log_debug(LOG_DEFAULT, ...)` throughout
- VSP bug fix: `page < 0xff` → `page <= 0xff` (vicii-cycle.c)
- Column count fix in vicii.c
- `vicii_init_colorram()` added (vicii-mem.c)
- `extern` keyword correctly applied to array declarations in vicii-mem.h
