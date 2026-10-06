## Final Build Games checkout notes

This is a fork of [RING's KarstNSim_Public](https://github.com/ring-team/KarstNSim_Public).
The default `main` branch imports upstream commit
[`ff55d396`](https://github.com/ring-team/KarstNSim_Public/commit/ff55d3969842f456908a6946c8bc8160a5d65965)
without code changes; this section adds local documentation. Work on the separate
`perf/scientific-memory` and `perf/compact-cave-graphs` branches is not included in
this default-branch checkout. The upstream overview, research citations, examples
and contact information are preserved below.

The current [CMake project](KarstNSim/CMakeLists.txt) requires **C++17**, overriding
the older C++14 requirement below, and declares CMake compatibility as
`3.8...3.28`. The following `-S`/`-B` commands require CMake 3.13 or newer; a 3.x
installation with a C++17 compiler and the selected generator's build tools is a
suitable starting point. Windows builds need Visual Studio's C++ workload and
Windows SDK; Linux builds need a compiler and Make or Ninja.

From the repository root:

```sh
cmake -S KarstNSim -B KarstNSim/build -DCMAKE_BUILD_TYPE=Release
cmake --build KarstNSim/build --config Release
```

For a Windows Visual Studio generator, add `-A x64` to the configure command.
Alternatively, run [build.bat](KarstNSim/build.bat) from `KarstNSim/`; it builds
both Debug and Release. Run a Windows example from the configuration directory:

```powershell
cd KarstNSim/build/Release
./karstnsim.exe ../../../Input_files/1_base/instructions.txt
```

The example's `main_repository: ../../..` and the interactive picker's
`../../../Input_files` are relative to the **working directory**. For a Linux
single-configuration build, the executable is `KarstNSim/build/karstnsim`.
The optional standard-library-only [Python launcher](parallel_launcher.py)
(Python 3.9+) can run it from the repository root, patching paths in generated
instruction copies:

```sh
python3 parallel_launcher.py --help
python3 parallel_launcher.py --exe KarstNSim/build/karstnsim --repo-root . --instructions Input_files/1_base/instructions.txt --seeds 1 --workers 1
```

Simulation runs write ASCII results and console logs under `outputs/`; the
launcher additionally writes instructions/logs under `batch_runs/`. It can run
the [four supplied examples](Input_files/) in sequence or across seeds. The CLI
[entry point](KarstNSim/src/main.cpp) parses an instruction file, then the
[simulation driver](KarstNSim/src/run_code.cpp) assembles/samples a graph, computes
geologically weighted paths and optionally amplifies the network or simulates
conduit dimensions. See [config_reference.md](config_reference.md) for inputs.
There is no CTest suite on this branch: the supplied examples are simulation
checks, and the CI workflow builds Linux/Windows targets. Compilation and simulation
commands above have not been run as part of this documentation refresh.

For API documentation, run `doxygen Doxyfile` from the repository root (with
Doxygen installed), then open `html/index.html`.
The code's [MIT license](LICENSE) names Université de Lorraine, ANDRA and BRGM;
Final Build Games maintains this fork, not the original research authorship.
The bundled [nanoflann header](KarstNSim/include/KarstNSim/nanoflann.hpp) retains
its own BSD notice. Please keep the upstream citation and attribution below.

---

# KarstNSim_Public
Public version of KarstNSim, a C++ code for graph-based and geologically-driven simulation of 3D karst networks.

* [2024 Publication](https://doi.org/10.1016/j.jhydrol.2024.130878)
* [2025 Thesis](https://hal.univ-lorraine.fr/tel-05114757v1)

Its inputs and outputs are ASCII files and it can be run through a single command.
It adapts the Karst simulation code proposed by <b> Paris, A., Guérin, E., Peytavie, A., Collon, P., Galin, E., 2021. Synthesizing Geologically Coherent Cave Networks. Comput. Graph. Forum 40, 277–287. https://doi.org/10.1111/cgf.14420 which is available on Github at : https://github.com/aparis69/Karst-Synthesis. </b>
This implementation includes modifications as compared to this initial independant version, in order to better suit geological data and information. 

The first version of KarstNSim was done in the frame of <b> Benoit Thebault </b> master's thesis, supervised by Pauline Collon. It was presented in the 2022 RINGMeeting in: <b> Thebault, B., Collon, P., Antoine, C., Paris, A., Galin, E., 2022. Karstic network simulation with γ -graphs, in: 2022 RING Meeting. </b>
From 2022 to 2025, KarstNSim has been developed in the frame of <b> Augustin Gouy</b>'s PhD thesis, supervised by Pauline Collon and Vincent Bailly-Comte.
From 2025 onwards, KarstNSim benefits from developments by Augustin Gouy.

The current version corresponds to KarstNSim 2.

It is recommended to read the methodology presented in the 2024 and 2026 articles and/or in the thesis to better apprehend the code.

If you use this code, please cite the associated article :

```
@article{Gouy2024,
author = {Gouy, Augustin and Collon, Pauline and Bailly-Comte, Vincent and Galin, Eric and Antoine, Christophe and Thebault, Beno{\^{i}}t and Landrein, Philippe},
doi = {10.1016/j.jhydrol.2024.130878},
issn = {00221694},
journal = {Journal of Hydrology},
month = {feb},
title = {{KarstNSim: A graph-based method for 3D geologically-driven simulation of karst networks}},
year = {2024}
}
```

## Changelog

Please find the link to the changelog [here](Changelog.md).

## Requirements

* [CMake](https://cmake.org/download/) 3.8 to 3.28 (select the Windows x64 Installer). During installation, check the option "add CMake to the system PATH".
* [Visual Studio 2017](https://my.visualstudio.com/Downloads?q=visual%20studio%202017&wt.mc_id=o~msft~vscom~older-downloads) or newer (for Windows).
* C++14 or newer (can be installed from Visual Studio).
* (Optional) [Doxygen 1.9.6](https://www.doxygen.nl/download.html) or newer for generating documentation.

## Compatibility

KarstNSim is designed to operate on Windows 10. While it hasn't been directly tested on Linux, there is an indication of compatibility based on a successful CMake test build.

## Installation

* Download the archive and unzip it somewhere (avoid spaces and special characters in the path).
* Go to the KarstNSim folder and run the batch file "build.bat", which will create a build folder and run CMake to generate build files and build the project (including compilation).
* An executable should have been generated in 'build/release/karstnsim.exe'.

**Running the code:**
- **Double-click** `karstnsim.exe`: the program scans `Input_files/` for subfolders and shows a numbered menu of available examples. Type the number of the example you want and press Enter.
- **Command line**
  ```bash
  cd path/to/your/executable
  karstnsim.exe ../../../Input_files/[path/to/your/instruction_file.txt]
  ```

*Make sure to use "/" or "\\\\" but never "\\" for the paths.*

Outputs are stored in the outputs directory.

## Documentation

### Input configuration documentation

If you're not looking to code in KarstNSim and only want to use it for simulation applications, you only need information on how to configure a simulation with the (many) inputs of KarstNSim. 
We provide a file in the archive ([here](config_reference.md)) which precises the type, theoretical and typical range of values, meaning, behavior and practical effect of all input parameters.

### Generate documentation files for full code documentation

If you're aiming at coding in KarstNSim or better understand how the code works, a doxyfile is present in the archive. To automatically generate the documentation, 
type `doxygen path/to/YourDoxyfile` in a command prompt, or simply `doxygen doxyfile` if already
 in the root folder. Once generated, you will find the documentation in the *hmtl folder*. It is advised to open the documentation starting from the main page, which provides important
general information about the project structure. You can find it by opening the "index.html" file, or by opening any other .html file and clicking on "Main Page" in the upper left.
A complete documentation of all user input parameters is available in the KarstNSim::ParamsSource struct page.

## Testing

We provide **four ready-to-run examples** (see the `Input_files/` folder). Each example lives in its own subfolder under `Input_files/`. When you double-click `karstnsim.exe`,
the interactive picker lists these folders and preselects the appropriate instruction file for each. The illustrations provided below have been generated with a slightly different version of KarstNSim,
meaning the seed will provide different random numbers and networks will be slightly different when using KarstNSim_Public.

> Tip: Use a 3D viewer such as [ParaView](https://www.paraview.org/download/) to inspect inputs/outputs.

### 1) Base simulation
A minimal, single-phase run on the synthetic dataset (as in Fig. 12 of the 2024 paper, but with some improvements). It demonstrates:
- vadose/phreatic partitioning,
- fracture guidance (two families),
- intrinsic karstification potential,
- inception surfaces.

<img src="base_example.png" alt="Base example" width="100%" align="center">

### 2) Polygenic karst (multi-phase reuse)
Reuses a previously generated network by **down-weighting already traversed edges**, mimicking multi-phase karstification (here, base-level drop with flow redirected to another outlet). The example disables the fracture term to emulate a more homogeneous medium in the bottom of the aquifer, and showcases **waypoints** and **karst-free points** close to the aquifer substratum.

<img src="polygenic_example.png" alt="Polygenic example" width="100%" align="center">

### 3) Amplification
Starting from the **base** network, this example runs the amplification step to control density and topology by adding:
- small-scale dead-end branches,  
- a limited number of cycles/loops.

<img src="amplification_example.png" alt="Amplification example" width="100%" align="center">

### 4) Karst section generation (curvilinear SGS)
Given the **amplified** network, this example simulates **equivalent radii on nodes** using a **curvilinear SGS** algorithm (Frantz et al., 2021). Variogram parameters mirror those in the paper; radii are visualized in log-scale and mapped to node sphere size for rendering. No external drift has been added.

<img src="karst_section_generation_example.png" alt="Karst section generation example" width="100%" align="center">

The main option of KarstNSim not presented in the above examples is the ghost-rock-driven simulation. Read the config reference file for information on correct use.


---


<b>For any information </b>, please contact : 
* Augustin Gouy : a.gouy.proaddress@gmail.com
* Pauline Collon : pauline.collon@univ-lorraine.fr 
* Christophe Antoine : christophe.antoine@univ-lorraine.fr

<b> To report a bug in the code </b>, please contact :
* Augustin Gouy : a.gouy.proaddress@gmail.com
Provide a screenshot of the log as well as complete information on your input parameters.