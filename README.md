# VR Iris

VR Iris is a small, private, social VR world built in Godot 4.7. You and a few friends meet in a temple and a city, with
physics you can grab and throw, voice chat, custom avatars and Iris (an AI who lives there). It runs on Windows PCs with a
PC-VR headset. There's also a desktop (mouse and keyboard) mode for PCs without one.

You can only get in **by invitation**. The game server sits behind a private network, so you need two things:
`VRIris-Setup.exe` from this release, and **your own invite file** (`yourname.irisinvite`), which Zeke sends you privately.

---

## Install (about 5 minutes)

1. **Put both files in one folder**, e.g. your Downloads: `VRIris-Setup.exe` and your `.irisinvite` file.
2. **Run `VRIris-Setup.exe`.**
   - **"Windows protected your PC" (blue box):** click **More info** and then **Run anyway**. The setup isn't code-signed
     (signing certificates cost money each year), so Windows doesn't recognise it yet. You'll see this once per release.
   - **"Smart App Control blocked this app"** (Windows 11 only): there's no Run anyway button. This feature blocks every
     unsigned program and has no per-app exception. Check *Windows Security ▸ App & browser control ▸ Smart App Control*.
     Most PCs that were upgraded from Windows 10 have it **Off** already. If yours is **On** or **Evaluation**, ask Zeke
     before you change it: on older Windows 11 builds, switching it off can't be undone without resetting Windows.
3. **Click through the setup:** Welcome → Folder → **Your invite** (filled in from your file) → **Private network**.
   - The private-network step installs **WireGuard**, the official free VPN app from wireguard.com, and makes **your own
     key on your PC**.
   - Windows asks **once** for administrator permission. The tunnel only carries traffic to the game server. The rest of
     your internet is untouched.
4. **"Send this code to Zeke":** setup shows a short code and has a **Copy** button. It's also saved on your desktop as
   `VR Iris - send this code to Zeke.txt`.
   - The code is safe to send anywhere (Discord, text). It's only the *public* half of your key; the secret half never
     leaves your PC.
   - Until Zeke has added it, the game says **"waiting for Zeke to approve your key"** and keeps retrying on its own. You
     can look around offline in the meantime.
5. **Finish → Start VR Iris.** The first start asks your name, avatar, height and comfort options (turning, movement) and
   shows the controller layout. Then it joins the temple.

Everything installs for **your Windows user only**, in `%LOCALAPPDATA%\Programs\VR Iris`. You get a desktop shortcut and
Start-menu entries: the game, desktop mode, your avatars folder and Uninstall. To uninstall, use *Settings ▸ Apps* or the
Start-menu entry.

### Before you put the headset on

VR Iris talks to your headset through **OpenXR**. On Windows, **one** VR app is the "OpenXR runtime" at a time, and the
setup's last page tells you which one your PC has.

| Headset | Start this first | Make it the OpenXR runtime |
|---|---|---|
| Meta Quest (cable / Air Link) | **Meta Quest Link** app on the PC | Link app ▸ Settings ▸ General ▸ OpenXR Runtime ▸ *Set as active* |
| Meta Quest (wireless) | **Virtual Desktop** Streamer (paid app) | Streamer ▸ OpenXR runtime ▸ *VirtualDesktopXR* |
| Valve Index, HTC Vive, Pico, others | **SteamVR** | SteamVR ▸ Settings ▸ OpenXR ▸ *Set SteamVR as OpenXR runtime* |
| Windows Mixed Reality | not supported: Microsoft removed it in Windows 11 24H2 | |

If no headset is found, the game starts in **desktop mode** and says what to fix. Start the VR app, connect the headset,
then start VR Iris again.

---

## Will my PC run it?

| | **Minimum** (VR, low preset) | **Recommended** (VR, high preset, 90 Hz) | Measured on |
|---|---|---|---|
| **GPU** | NVIDIA GTX 1060 6 GB / AMD RX 480 / RX 580 8 GB (Quest Link: RTX 20-series or newer, see below) | NVIDIA RTX 3060 / RTX 2070 / RTX 4060 · AMD RX 6600 XT / RX 7600 · Intel Arc A750 | ✔ RTX 3060 12 GB |
| **VRAM** | 6 GB | 8 GB or more | (6 GB = the game's memory budget floor) |
| **CPU** | 4 cores / 4 threads, e.g. Intel i5-4590 / Ryzen 5 1500X | 8 threads or more, e.g. Ryzen 5 3600 / i5-10400 or newer | ✔ Ryzen 7 3700X (8 cores / 16 threads) |
| **RAM** | 8 GB | 16 GB | ✔ 32 GB |
| **Disk** | 1.5 GB free (the game is 0.5 GB, plus the shader cache and the setup file) | SSD | ✔ |
| **Windows** | Windows 10 22H2 (64-bit) | Windows 10 22H2 or Windows 11 | ✔ Zeke's PC (Windows 10 22H2) · ✔ a Windows 10 Home 22H2 test VM: install, private network, desktop mode · Windows 11: **not tested yet** |
| **Headset** | any OpenXR PC-VR headset: Quest 2/3/3S/Pro via Link, Air Link or Virtual Desktop; Index; Vive; Pico | Quest 3 on a USB-3 Link cable or Wi-Fi 6 | ✔ Zeke's headset (daily play) |
| **Desktop mode** (no headset) | any GPU with Vulkan 1.0, Direct3D 12 or OpenGL 3.3 (integrated graphics are fine) | | ✔ a VM with no GPU at all (software OpenGL, slow but works) |

**Measured or estimated:**

- **Measured on Zeke's PC** (Ryzen 7 3700X, 16 threads, 32 GB, RTX 3060, Windows 10 22H2), using our automated client test
  (`vr.sh clientsmoke`, 3 runs, this release's build):
  - The loading screen is up after **9 s** and the world is ready after **27–28 s**.
  - Desktop mode then holds **~185 fps** at the spawn, with **~6 ms** of game logic and **~3 ms** of physics per frame.
  - In VR this PC runs the **high** preset at **90 Hz**. That comes from Zeke's daily play, not from this automated test.
- **Estimated, not measured:** everything else in the minimum and recommended columns. We have no GTX 1060, AMD or Intel
  card to test on. The estimates come from the published minimums of comparable PC-VR games and runtimes (table below),
  scaled to how heavy VR Iris is on the RTX 3060.
- **Meta Quest Link has its own GPU list** (link in the sources). It supports NVIDIA RTX 20/30/40/50; check the list for
  AMD cards. It does **not** support Intel Arc, workstation cards, the GTX 1050 Ti or the RX 560, and "doesn't recommend" the
  laptop RTX 3050 / 3050 Ti. With an unsupported card, use **Virtual Desktop** or SteamVR instead.

| Published PC-VR specs (for comparison) | CPU | GPU | RAM |
|---|---|---|---|
| SteamVR minimum / recommended | i5-4590 / FX 8350 | GTX 970 / R9 290 · rec. GTX 1060 / RX 480 | 4 GB |
| Meta Quest Link minimum / recommended | i5-4590 / Ryzen 5 1500X · rec. i7 / Ryzen 7 | supported list · rec. RTX 20 / RX 6000 | 8 GB · rec. 16 GB |
| Half-Life: Alyx minimum | i5-7500 / Ryzen 5 1600 | GTX 1060 / RX 580 6 GB | 12 GB |
| Blade & Sorcery minimum / recommended | quad-core 3 GHz · rec. 8-core 4 GHz | GTX 1070 · rec. RTX 2070 | 8 GB · rec. 16 GB |

### Graphics settings pick themselves

At the first start the game looks at your graphics card, CPU and RAM and picks a preset: **low, medium, high or ultra**
(render scale, anti-aliasing, mirror quality).

| Your PC | Preset |
|---|---|
| RTX 3080 / 4070 / 5070 and up · RX 6800 / 7800 / 9070 and up | ultra |
| RTX 3060 / 3060 Ti / 3070 / 2060 Super / 2070 / 4060 / 5060 · RX 6600–6750 / 7600 / 7700 / 9060 · Arc A750/A770/B580 | high |
| 6 GB cards (GTX 1660, RTX 2060, laptop RTX 3060), GTX 1070/1080, RX 5600/5700, laptop RX 6600M · a card the game doesn't know | medium |
| GTX 1060 / 1650 / 1050 / 9xx, RX 4xx/5xx/6500, Arc A380 · integrated graphics · no graphics driver | low |

- **Laptop GPUs** count one step lower than the desktop card with the same name.
- **CPU under 8 threads, or RAM under 16 GB:** at most medium.
- **Under 4 threads or 8 GB:** low.

You can change any of it in *Settings ▸ Graphics*. Once you've changed something by hand, it stays yours.

---

## Troubleshooting

| Problem | What to do |
|---|---|
| **"waiting for Zeke to approve your key"** | Send Zeke the code from `VR Iris - send this code to Zeke.txt` on your desktop (also in `tools\my_public_key.txt` in the game folder). The game connects by itself once it's added. If it **used to work** and now says *"the temple isn't answering"*, the server is probably just offline. |
| **Game runs in desktop mode, but you have a headset** | Start your VR app first and make it the OpenXR runtime (see the table above), then restart the game. |
| **The loading screen takes long** | The first start compiles shaders (up to a minute or two on slower PCs) and later starts are quicker. If it says *"Slower than usual"*, it lists what it's doing. |
| **Frozen / hangs** | Press **F9** for the debug panel and **F10** to copy diagnostics (they go to the clipboard and into a file in `%APPDATA%\Godot\app_userdata\VR Iris`), then paste them to Zeke. If a start freezes, the next start switches to the **Direct3D 12** renderer on its own and the debug panel says so. |
| **Parts of the world invisible or flickering (AMD)** | A known AMD Vulkan driver bug: *Settings ▸ Graphics ▸ Renderer → DirectX 12*, then restart. Updating the AMD driver also helps. |
| **Black window or crash at the first start** | Update your graphics driver from nvidia.com, amd.com or intel.com. Windows Update's driver is often old. If Vulkan can't start at all, the game tries Direct3D 12 and then OpenGL by itself. |
| **Laptop runs badly** | Plug in the charger and set the Windows power mode to *Best performance*. The game already asks for the gaming GPU, not the integrated one. |
| **Antivirus says it's a virus** | Small unsigned installers and Godot games sometimes get flagged by mistake. The files come only from this release page, and the `.sha256` file next to Setup lets you check yours. Tell Zeke which antivirus flagged it, and we'll report the false positive. |
| **The private network step failed** | Run Setup again with your invite file; it keeps your key. The reason is in `tools\wg_last.txt` in the game folder (no secrets in it). Other VPN apps can get in the way, so pause them during setup. |
| **Firewall asks about VR Iris** | The game only connects outward, so it doesn't need incoming access. Either answer is fine. |
| **Reinstalling** | Uninstalling asks whether to remove the private-network tunnel. Say **No** if you'll reinstall: removing it deletes your key, and Zeke would have to approve a new code. |

**Not needed:** no Visual C++ or DirectX redistributable and no .NET. The game is a single Godot 4.7 executable with two
DLLs.

---

## Sources

The specs and fixes above come from these, checked 2026-11-03:

- Godot system requirements (Windows 10+, Vulkan 1.0 / D3D12 12_0 / OpenGL 3.3): https://docs.godotengine.org/en/stable/about/system_requirements.html
- Godot renderer fallback (Vulkan ⇄ D3D12 → OpenGL 3, since 4.4; verified in the engine's own defaults): https://docs.godotengine.org/en/stable/tutorials/rendering/renderers.html
- Godot recommends Vulkan for XR on Windows: https://godotengine.org/article/godot-xr-update-mar-2026/
- Godot asks for the dedicated GPU on Optimus / PowerXpress laptops (`NvOptimusEnablement`): https://github.com/godotengine/godot/blob/4.5/platform/windows/os_windows.cpp
- AMD Vulkan invisible meshes after a driver update, D3D12 works around it: https://forum.godotengine.org/t/godot-editor-incapable-of-rendering-3d-graphics-windows-11-amd/118821
- SmartScreen "More info → Run anyway", reputation per file hash: https://textslashplain.com/2024/11/15/best-practices-for-smartscreen-apprep/
- Smart App Control, no per-app exception: https://support.microsoft.com/en-us/windows/smart-app-control-frequently-asked-questions-285ea03d-fa88-4d56-882e-6698afdb7003
- NSIS false positives: https://nsis.sourceforge.io/NSIS_False_Positives · Microsoft false-positive submission: https://www.microsoft.com/wdsi/filesubmission
- OpenXR active runtime (`HKLM\SOFTWARE\Khronos\OpenXR\1\ActiveRuntime`): https://github.com/KhronosGroup/OpenXR-SDK-Source/blob/main/specification/loader/runtime.adoc
- Meta Quest Link PC requirements and supported GPUs: https://www.meta.com/help/quest/140991407990979/
- Meta Link "set as active OpenXR runtime": https://developers.meta.com/horizon/documentation/unity/unity-link
- Virtual Desktop OpenXR (VDXR): https://github.com/mbucchia/VirtualDesktop-OpenXR
- Windows Mixed Reality removed in Windows 11 24H2: https://www.pcworld.com/article/2490448/windows-11s-2024-update-finally-abandons-this-once-promising-feature.html
- Windows 10 end of support (still runs): https://support.microsoft.com/en-us/windows/windows-10-support-has-ended-on-october-14-2025-2ca8b313-1946-43d3-b55c-2b95b107f281
- Windows Firewall prompts come from listening apps; outbound is allowed by default: https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/rules
- WireGuard for Windows silent install and tunnel services: https://github.com/WireGuard/wireguard-windows/blob/master/docs/enterprise.md
- SteamVR specs: https://store.steampowered.com/app/250820/SteamVR/ · Half-Life: Alyx: https://store.steampowered.com/app/546560/HalfLife_Alyx/ · Blade & Sorcery: https://store.steampowered.com/app/629730/Blade_and_Sorcery/
