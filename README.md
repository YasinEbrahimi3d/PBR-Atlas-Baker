PBR Atlas Baker

A Blender add-on that bakes **Base Color / Roughness / Metallic / Normal /
Displacement** from any number of selected objects onto one shared UV atlas,
merges the results into a handful of combined PNG textures, and wires them
into a single new shared material — automatically.

Instead of manually creating a new UV map, packing it, baking each object,
and combining textures by hand, this add-on does the whole pipeline in one
click.

## Features

- Bakes multiple objects into one shared texture atlas (instead of one
  texture per object).
- Supports up to 5 channels: Base Color, Roughness, Metallic, Normal,
  Displacement.
- Automatically creates and packs a new UV map without touching your
  original UV layout.
- Produces at most 5 final PNG files, no matter how many objects you baked.
- Builds a ready-to-use shared material and assigns it to every baked
  object.
- Adjustable resolution (512–4096) and UV pack margin.

## Requirements

- Blender 4.2 or newer (older versions may work but are not officially
  tested).
- A GPU or CPU capable of running Cycles.
- NumPy, which ships with Blender's own Python by default.

---

## 1. Installation


1. Download the add-on's `.zip` file (do not unzip it).
2. In Blender, go to **Edit > Preferences > Add-ons**.
3. Click **Install...** and select the downloaded `.zip` file.
<img width="4796" height="4246" alt="TutorialPage_01" src="https://github.com/user-attachments/assets/3fbb94a0-a6f5-4f0e-9eef-834cfeed563a" />

4. Enable the checkbox next to **PBR Atlas Baker** to activate it.
<img width="3739" height="2805" alt="TutorialPage_02" src="https://github.com/user-attachments/assets/a89ea2bb-4d14-4f6f-8479-1e8452146109" />

5. In the 3D Viewport, press **N** to open the sidebar, then click the
   **Atlas Baker** tab. The panel should now be visible.
<img width="4844" height="4500" alt="TutorialPage_03" src="https://github.com/user-attachments/assets/6f92dc5c-6938-46ad-a498-6505713089ef" />


---

## 2. Usage


1. Select the objects you want to bake. Every object must be a **Mesh**
   with an existing UV map and a node-based material.
 <img width="6107" height="4500" alt="TutorialPage_04" src="https://github.com/user-attachments/assets/7d9fe438-f42a-4c2e-968f-d7bf78af7d6b" />
  

2. In the panel, set:
   - **Output Directory** — where the final PNGs will be saved.
   - **New UV Name** — name of the atlas UV map that will be created.
   - **File Base Name** — prefix used for the output file names.
   - **Resolution** — 512, 1024, 2048, or 4096.
   - **UV Pack Margin** — spacing between UV islands.
<img width="6270" height="3659" alt="TutorialPage_05" src="https://github.com/user-attachments/assets/c52f15bb-ad61-427b-ae9b-9ecbb1284042" />


3. Tick the channels you want baked: **Base Color**, **Roughness**,
   **Metallic**, **Normal**, **Displacement**.
<img width="6270" height="3659" alt="TutorialPage_06" src="https://github.com/user-attachments/assets/6881ec8d-058f-490a-b9ea-25df3cb69d1f" />


4. (Optional) Enable **Also Keep Per-Object Bakes** if you also want each
   object's individual bake saved to disk, in addition to the combined
   atlas. Off by default.
<img width="6270" height="3659" alt="TutorialPage_07" src="https://github.com/user-attachments/assets/29825b16-6344-4a95-9854-9bd6861cec24" />


5. Click **Bake Atlas** and wait for the process to finish. Progress can be
   seen at the bottom-right of the Blender window.
<img width="6270" height="3659" alt="TutorialPage_08" src="https://github.com/user-attachments/assets/bd0faa45-7f20-417a-98b8-6ed9a1e5a14c" />


6. When it's done, check your output folder — you'll have up to 5 combined
   PNG files (one per channel), e.g. `Atlas_BaseColor.png`,
   `Atlas_Roughness.png`, `Atlas_Normal.png`.
<img width="7555" height="3513" alt="TutorialPage_09" src="https://github.com/user-attachments/assets/2cbd70c1-e069-4ad8-9380-f5f56176e8d8" />


7. All baked objects are automatically assigned one new shared material
   that reads the combined textures through the new atlas UV.
<img width="4212" height="4500" alt="TutorialPage_10" src="https://github.com/user-attachments/assets/bed129be-eab3-4b96-a83a-7f7216511cd1" />


---

## How it works internally

- Each object keeps its original UV map untouched; a second, new UV map is
  created alongside it and packed together with every other selected
  object's new UV, into one shared 0–1 atlas space.
- Baking reads source textures through the **original** UV (so texture
  content stays correct) while writing results into the **new, packed**
  UV (so islands land in the right place in the atlas).
- Base Color / Roughness / Metallic are baked only if directly connected to
  the matching Principled BSDF input.
- Normal is only recognized through the standard
  Principled → Normal Map → Image Texture chain, and baked with Blender's
  NORMAL bake type.
- Displacement is read from whatever feeds the Material Output's
  Displacement socket, even through several intermediate nodes.
- Every object's bake is merged into a running combined canvas and freed
  right away, so memory use stays roughly constant regardless of how many
  objects you bake.

## Known limitations

- Baking requires Cycles; the render engine is switched automatically and
  restored afterwards.
- Large resolutions (2048/4096) combined with many objects will take
  noticeably longer and use more GPU/CPU resources.
- Always try it on a copy of your scene first.

## Credits & Disclosure

This add-on was developed with the assistance of AI (Claude, Anthropic) for
writing and debugging the code, under the direction and testing of the
author. Every part of the logic — UV handling, baking, and the memory
optimizations described above — was reviewed, tested, and fixed through
several rounds of real-world testing in Blender before being published
here. The source code is fully open and included in this repository, so
you can read through it yourself before installing.
