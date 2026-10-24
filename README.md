<!--    
    ================================================================================================
    
    Copyright (c) 2026 Ilya Snegov (aka Sierra Arn)
    
    Dedicated to the public domain under the Creative Commons
    CC0 1.0 Universal Public Domain Dedication (the "Dedication").
    You may obtain a copy of the Dedication at
    
    https://creativecommons.org/publicdomain/zero/1.0/legalcode.en

    ================================================================================================
    README.md
    ================================================================================================
-->

# Audio Forge for Opus One

> *A Pixi workspace for building publication-ready audio files of «Sierra Arn — Composizioni, Op. 1» from MuseScore sources.*

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](#license)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](#license)
[![License: CC0 1.0](https://img.shields.io/badge/License-CC0%201.0-blue.svg)](#license)

> [!NOTE]  
> This repository is part of a cohesive trilogy of projects designed to transform raw MuseScore files into publication-ready multimedia formats using fully reproducible environments. Each project handles a specific stage of the pipeline:
>
> | Repository | Focus | Result |
> | :--- | :--- | :--- |
> | [**Score Forge Opus One**](https://github.com/snegoviol/score-forge-opus-one) | Generating publication-ready PDF scores | [PDF Release](https://github.com/snegoviol/score-forge-opus-one/releases/tag/v2026.09.04) |
> | [**Audio Forge Opus One**](https://github.com/snegoviol/audio-forge-opus-one) <br>*(you are here)* | Generating publication-ready audio files | [Audio Release](https://github.com/snegoviol/audio-forge-opus-one/releases/tag/v2026.09.15) |
> | [**Video Forge Opus One**](https://github.com/snegoviol/video-forge-opus-one) | Generating publication-ready video files | [▶ YouTube Playlist](https://youtube.com/playlist?list=PLAbmQ-iQqaoo&si=DQ8Rovb4TtuZEolb) <br> [Video Release](https://github.com/snegoviol/video-forge-opus-one/releases/tag/v2026.09.15) |

> [!WARNING]  
> **Project Archived**
>
> This project is no longer maintained. Issues and pull requests are not accepted, so please fork the repository if you want to build on it. See the sections below for details.
> 
> **Context:**  
> Originally, the primary goal of this project was to transform raw MuseScore files into publication-ready music. While this could have been done manually, my passion for both music composition and software engineering led me to wrap the entire pipeline in fully reproducible environments. This endeavor served as a practical deep-dive into `rattler build` (via `pixi-build`), allowing me to package complex dependencies from source and orchestrate a robust media pipeline.
> 
> **Reason for Archiving:**  
> With the musical works now fully composed and published, the primary objective of this repository has been successfully achieved, and active development is no longer required. However, this repository remains publicly available as a valuable resource for:
> - Musicians and producers interested in remixing or studying the underlying composition files.
> - Programmers looking to bridge the gap between software engineering and music production.
> - Developers exploring `rattler build` who need a complex, real-world example of building local conda packages and automating media workflows.

<!--    
    ================================================================================================
-->

## Project Structure at a Glance

```
audio-forge-opus-one/
├── composizioni/       # Musical workspaces for `Composizioni, Op. 1`. 
│                       # Each `Op-1_No-*/` subdirectory represents one musical
│                       # piece.
│
├── recipes/            # Local conda packages built via `pixi-build` and 
│                       # declared as workspace dependencies. Each subdirectory
│                       # represents one local conda package recipe.
│
├── docs/               # Technical documentation covering workspace dependencies,
│                       # and detailed project structure.
│
├── LICENSES/           # Full texts of all licenses used in the project.
│
├── LICENSE.txt         # Licensing overview: which license applies to which files.
│
├── metadata.toml       # Shared release metadata (used by workspace CLIs).
│
├── pixi.toml           # Workspace manifest: channels, dependencies, and tasks.
│
└── pixi.lock           # Fully resolved and reproducible dependency lockfile.
```

<!--    
    ================================================================================================
-->

## Quick Start

### I. Prerequisites

| Component            | Requirement                               |
| :------------------- | :---------------------------------------- |
| **Package Manager**  | [`Pixi`](https://pixi.prefix.dev/latest/) |
| **Operating System** | GNU/Linux-based system                    |
| **Architecture**     | `x86_64`                                  |

> **Note:**  
> These are not strict requirements, but rather the baseline environment where the code is 
guaranteed to run out of the box. Alternative setups are possible but may require manual 
configuration.

<!--    
    ================================================================================================
-->

### II. Setup

1. **Clone the repository**

    ```bash
    git clone https://github.com/snegoviol/audio-forge-opus-one.git
    cd audio-forge-opus-one
    ```

2. **Install dependencies**

    ```bash
    pixi install
    ```

    > **Note:**  
    > 1. MuseScore is built from source during `pixi install`; this can take a considerable amount of time.
    > 2. Many compiler warnings while building MuseScore are expected and can be ignored.

3. **Activate environment**

    ```bash
    pixi shell
    ```

4. **Fetch MuseScore sources from [score-forge-opus-one](https://github.com/snegoviol/score-forge-opus-one)**

    ```bash
    pixi run fetch-mscz-scores
    ```

<!--    
    ================================================================================================
-->

### III. Build

With the environment activated and the MuseScore sources in place, publication-ready audio files can be rebuilt in the following steps:

1. **Launch MuseScore**

    ```bash
    musescore
    ```

2. **Export each score to FLAC through the MuseScore GUI**

    For each piece under `composizioni/Op-1_No-*/`:

    1. Open `score.mscz`.

        > **Note:**  
        > Opening a score may print many FluidSynth lines of the form  
        > `SoundFont(.../MuseScore_General.sf3) Sample(...) fowled (broken soundfont?), fixing up`  
        > (often for pizzicato samples such as `BassPzz_*`, `CellPzz_*`, `ViolPzz_*`, `VlnsPzz_*`).  
        > These messages are expected and can be ignored if MuseScore opens and plays back without crashing.

    2. Choose **File -> Export**.
    3. Set **Export to** to **FLAC Audio**.
    4. In the **What to export** block:

        - For every piece except *«No. 3: Abyss»* and *«No. 5: To the Beloved»*: check **Full Score** only.
        - For *«No. 3: Abyss»* and *«No. 5: To the Beloved»*: uncheck **Full Score**, and instead check every listed part (for example **Piano**, **Harp**, **Soprano**, **Contralto**).

    5. In the **export options** block, set:

        | Setting | Value |
        |---|---|
        | Normalize | **Off** (unchecked) |
        | Sample rate | **48000 Hz** |

    6. Save the output into the same piece directory.

        > **Note:**  
        > MuseScore names the written files from the **What to export** selection:
        > - If **Full Score** only is checked, it writes a single `score.flac`.
        > - If individual parts are checked instead, it writes one file per part as `score-<Part>.flac`  
        > (for example `score-Piano.flac`, `score-Soprano.flac`).

<!--    
    ================================================================================================
-->

3. **Close MuseScore**

    Close the application via the window close button in the MuseScore GUI.

4. **Apply send/return reverb for every piece into `reverbed.flac`**

    ```bash
    pixi run reverb-all
    ```

5. **Apply constant gain to `reverbed.flac` into `normalized.flac` for every piece**

    ```bash
    pixi run normalize-loudness-all
    ```

6. **Create `release.flac` from `normalized.flac` with release metadata for every piece**

    ```bash
    pixi run release-all
    ```

> **Want to see what happens under the hood?**  
> The Pixi tasks that drive this pipeline are defined here:
> - [Workspace tasks](./pixi.toml)
>
> Those tasks invoke the audio pipeline CLIs. Every file is fully documented with detailed docstrings:
> - [Pipeline CLIs](./recipes/scripts/src/scripts/)

<!--    
    ================================================================================================
-->

## License

Each file in this repository is licensed under one of the following licenses:

- [Apache License, Version 2.0](LICENSES/Apache-2.0.txt),
- [Creative Commons Attribution 4.0 International Public License](LICENSES/CC-BY-4.0.txt),
- [Creative Commons CC0 1.0 Universal Public Domain Dedication](LICENSES/CC0-1.0.txt).

The applicable license is stated in the header comment of each file. 
See [LICENSE.txt](LICENSE.txt) for details.

<!--    
    ================================================================================================
-->
