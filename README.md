# Description and results

University project for the '3D Graphics' subject where we built a rendering engine in OpenGL. It was developed incrementally; the folder 'P4OGL' contains the final version, which features deferred shading.

The engine evolved over multiple core milestones, transforming from a foundational modular graphics application into a high-performance, feature-rich renderer supporting advanced lighting models, physical materials, custom mesh processing, and screen-space post-processing.

### Key Features & Milestones Achieved
* **Asset Loading via Assimp:** integrated via `vcpkg` package manager
  
* **Camera movement:** fully interactive view-matrix system supporting:
  * A FPS keyboard-steered camera controller.
  * An orbital camera system using mouse-drag.
    
* **Aspect-Ratio Preservation.**

  <img width="400" alt="resizing" src="https://github.com/user-attachments/assets/6536a819-d6eb-4a9f-b62a-8008305ee5b8" />

  
* **Multiple light sources:** dynamic real-time lighting with concurrent multiple light sources. Built configurations for Directional lights (parallel rays) and customizable Spotlights (defined by aperture angles and falloff thresholds), utilizing distance-based illumination attenuation functions (Unreal Engine approach) for improved visual realism.

<table style="border: none; border-collapse: collapse;">
  <tr style="border: none;">
    <td style="border: none; padding: 5px;">
      <img width="300" alt="two lights" src="https://github.com/user-attachments/assets/dba6daf0-6f39-4ae5-b31f-3552acb6852e" />
    </td>
    <td style="border: none; padding: 5px;">
      <img width="250" alt="focal" src="https://github.com/user-attachments/assets/8d963143-155d-42fd-ae14-10b680c2b902" />
    </td>
  </tr>
</table>

* **Fragment Discard & Procedural Shading:** implemented shaders with fragment discarding for transparency patterns, alongside custom procedural noise generation algorithms.
  
* **Disney 2012 BRDF Model:** Implemented Disney's principled Physically-Based Rendering (PBR) model with most features, including *Sheen*, using Schlick's approximation for fabric/velvet micro-fibers, and GTR1 distributions to simulate glossy *Clearcoat* varnish layers.

<table style="border: none; border-collapse: collapse;">
  <tr style="border: none;">
    <td style="border: none; padding: 5px;">
      <img width="250" alt="clearcoat" src="https://github.com/user-attachments/assets/7153acf5-eec9-485c-a806-02267f9b5ece" />
    </td>
    <td style="border: none; padding: 5px;">
      <img width="270" alt="sheen" src="https://github.com/user-attachments/assets/be7e0a31-2749-4945-9837-277192d141ed" />
    </td>
  </tr>
</table>

* **Tangent-Space Bump Mapping.**
<img width="300" alt="bump" src="https://github.com/user-attachments/assets/e11d06d9-3c5f-4736-bee9-66d6e2755915" />




* **G-Buffer / Deferred Shading.**
* **Advanced Post-Processing Chain:**
  * **Motion Blur:** using alpha blending accumulation.
  * **Depth of Field (DoF):** hardware depth textures (`depthTex`) mapped to view-space coordinates to calculate cinematic focal distances and variable defocus blurs.
  * **Multi-Pass Filters:** optimized 2-pass horizontal and vertical Gaussian Blur.
<img width="300" alt="blur" src="https://github.com/user-attachments/assets/15be7523-ca9d-446a-805b-2639b723a100" />


# Instructions

## Windows

### Dependencies

We previously need [Git](http://git-scm.com/install/windows), [CMake](https://cmake.org/download/) (make sure to check the **Add CMake to the system path** option) and [Visual Studio](https://visualstudio.microsoft.com/es/vs/community/) (make sure to include **Desktop development with C++** in the Visual Studio installation) installed in our PC.
Go to C:\Users\<Your user name>, and open PowerShell from the searchbar. We install the required dependencies via PowerShell:
```powershell
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
```
Integrate it with Visual Studio/CMake and install the required packages:
```powershell
.\vcpkg integrate install
.\vcpkg install glew freeglut glm freeimage
```

Install assimp for optional part
```powershell
.\vcpkg install assimp
```

### Building
```powershell
cd <project_root_folder>
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Debug -A x64 -DCMAKE_TOOLCHAIN_FILE=C:/Users/<Your user name>/vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build . --config Debug --parallel 10
```

### Running

We need to declare a temporal environment variable to be able to run the application (this variable will only exists in that PowerShell session):
```powershell
cd <project_root_folder>/build
cd p1GLSL\Debug
$env:Path += ";C:\Users\<Your user name>\vcpkg\installed\x64-windows\bin"
.\p1GLSL
```

## Linux (Ubuntu)

### Dependencies
```
sudo apt install build-essential cmake cmake-curses-gui git libxmu-dev libxi-dev libgl-dev  libglew-dev libfreeimage-dev freeglut3-dev
```

### Building
```bash
cd <project_root_folder>
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Debug
make -j10
```

### Running

Make sure the shader files and images are accesible to the executables copying the related files into: `<project_root_folder>/build`. After you should have an structure like this:
```
- build
-- img -> contaings images
-- p1GLSL -> contains project executable
-- shaders_p1 -> contains shaders for exercise 1
```

Then, go to the building folder:

```bash
cd <project_root_folder>/build
./p1GLSL/p1GLSL
```
