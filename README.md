# NFS: Most Wanted (2005) on Android

A practical guide for running **Need for Speed: Most Wanted (2005)** on Android: **mid-range** (e.g. **Realme 9 Pro / Snapdragon 695**) and **high-end** (e.g. **POCO F3 / F4 / F5 / F6** with Snapdragon **8-series**). Use it when you reinstall the setup or help someone else get it working.

### Quick links

- [Winlator releases](https://github.com/brunodev85/winlator/releases)
- [ThirteenAG Widescreen Fixes Pack releases](https://github.com/ThirteenAG/WidescreenFixesPack/releases) (NFS MW fix)
- [NFS: Most Wanted Black Edition — Internet Archive](https://archive.org/details/nfs-mw-be)

---

## Phase 1 — File preparation (Ubuntu)

1. **Get the game**  
   Use the **Black Edition** from the Internet Archive: [NFS Most Wanted Black Edition (`nfs-mw-be`)](https://archive.org/details/nfs-mw-be).

2. **Extract / install**  
   The release uses a newer Inno Setup. On Ubuntu, run `wine NFSMW.exe` to install into a folder.

3. **Widescreen fix**  
   From [ThirteenAG Widescreen Fixes Pack releases](https://github.com/ThirteenAG/WidescreenFixesPack/releases), download the **NFS: Most Wanted** fix. Copy the `scripts` folder and `dinput8.dll` into the game root (next to `speed.exe`).

4. **Zip for transfer**  
   Zip the whole game folder. This avoids slow ~1 MB/s Linux→Android MTP transfers.

---

## Phase 2 — Moving to the phone

1. **Transfer**  
   Copy the `.zip` to the phone’s **Download** folder over USB.

2. **Extract on device**  
   Install **ZArchiver** from the Play Store. Create **`/Games/`** on internal storage and extract the archive there.

**Why `/Games/`?** On Android 13+, the Download folder is restricted for many apps; a dedicated folder like `Games` usually works without that hassle.

---

## Phase 3 — Winlator container (Realme 9 Pro, Snapdragon 695)

Install **Winlator** from [GitHub releases](https://github.com/brunodev85/winlator/releases), then create a container with the settings below.

| Setting | Value |
|--------|--------|
| **Graphics driver** | Turnip (Adreno) — best fit for Snapdragon |
| **DX wrapper** | WineD3D (Legacy) — use if DXVK gives a silent crash or black screen |
| **Windows version** | Windows XP |
| **CPU affinity** | Disable CPUs **0** and **1**; enable **2–7** (performance cores) |
| **Video memory** | 2048 MB |
| **DX components** | DirectInput and DirectSound → **Native (Windows)** |

---

## Phase 4 — Library overrides (Widescreen fix)

If the game won’t start, force Wine to use the fix libraries:

1. Start the container → **Start → System Tools → Wine Configuration**.
2. Open the **Libraries** tab.
3. Add **`dinput8`** and **`d3d9`**.
4. Set both to **(Native, Built-in)**.

---

## Phase 5 — Controller and gamepad

- **Realme / OTG**  
  For a wired controller: enable **OTG** in phone settings, or the phone may not power the pad.

- **Winlator input**  
  **Side menu → Input Controls** → create a profile mapping gamepad buttons to **arrow keys** (gas, brake, left, right) and **Space** (handbrake).

- **Analog steering**  
  In the game’s `scripts` folder, edit `NFSMostWanted.WidescreenFix.ini` and set `ImproveGamepadSupport = 1`.

---

## Troubleshooting (quick)

| Issue | What to try |
|--------|-------------|
| Game won’t start | Switch DX wrapper to **WineD3D**; recheck **library overrides** (Phase 4). |
| Low FPS | Lower in-game resolution (e.g. **800×600**); turn off **shadows**. |
| Controls dead | In Winlator’s side menu, select your **custom input profile** after the game is running. |

---

## POCO phones — Snapdragon 8-series (F3, F4, F5, F6)

Many POCO devices ship with fast **Snapdragon 8-series** SoCs, so **Winlator usually runs smoother** than on a mid-range Realme — but you may see **black-screen** behaviour, **MIUI / HyperOS quirks**, or **high heat**.

### Black screen on the second launch (HyperOS / MIUI)

The game may run once, then on the next open you get a **black screen with audio only** (the system can “hang on” to a stale video buffer).

**Fix — wrapper reset**

1. Set **DX wrapper** to **CNC DDraw** or **WineD3D**.
2. Start the game until the **menu** is visible, then **exit cleanly**.
3. Switch **back to DXVK** and launch again.

**Alternative — offscreen mode**

In **container settings**, set **Offscreen rendering mode** from **FBO** to **Backbuffer**.

### GPU driver (POCO F5 / F6, Adreno 7xx)

Adreno **7-series** can be picky about Turnip builds.

- Prefer **Turnip v24.3.0** or **v26.x.x** (newer is not always better for this title).
- If you see **texture flicker**, try an **older Turnip** version in Winlator’s driver list.

### CPU affinity (reduce stutter under throttling)

POCO firmware is **aggressive about thermals**; letting the scheduler bounce work onto little cores can cause **stutter**.

In the container’s **CPU affinity** settings:

- **Uncheck** CPUs **0, 1, 2, 3** (efficiency cores).
- **Check only** CPUs **4, 5, 6, 7** (performance cores).

### Resolution soft-lock (crash on launch)

If the game **crashes immediately** on a POCO:

1. Run **`NFSMW_VideoConfig.exe`** in the game folder.
2. Set **640×480**, save, and launch once.
3. After you are **in-game**, use the **Widescreen Fix** / in-game options to move up to your target resolution (e.g. native).

---

## Realme vs POCO — quick reference

| Feature | Realme 9 Pro | POCO F5 / F6 |
|--------|----------------|----------------|
| **DX wrapper** | WineD3D (Legacy) | DXVK **1.10.3** or **2.4** (use CNC DDraw / WineD3D to break black-screen loops, then return to DXVK if needed) |
| **GPU driver** | Turnip (Adreno) | Turnip **v24+** tuned for **Adreno 7xx** |
| **Box64 preset** | Compatibility | Performance / Intermediate |
| **Rendering** | FBO | **Backbuffer** (helps prevent black screen on some MIUI / HyperOS builds) |

---

## Extra tip (Realme 9 Pro)

PC emulation pushes the **Snapdragon 695** hard. **Play with the case off** when you can — heat is the main limit.

---

*Good luck taking the #1 spot on the Blacklist. Drive safe.* 🏎️💨
