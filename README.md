# Glue3d+: a more optimized engine with more functionality than any other version of Glue3d.

# Engine Update: Performance Overhaul & Feature Expansion

This Mod introduces massive performance improvements, structural refactoring, and new features to the game engine. A specific focus was placed on optimizing execution for the **TurboWarp runtime**, reducing overhead in the custom interpreter, and minimizing project file size. Version 1.6 marks a major milestone with the introduction of a scene management system and more flexible editing tools.

Here is the detailed breakdown of all modifications:

## ⚡ Performance Optimizations (Math & Loops)
*   **TurboWarp Math Exploitation:** Replaced divisions with fractional multiplications to better exploit the TurboWarp engine's JIT compilation.
*   **Infinity Bypass:** Replaced `-1 / 0` operations directly with `-Infinity` to bypass division overhead.
*   **Loop Hoisting:** Pre-calculated static values like the Mode7 scale and `RenderDistance` outside of main loops to avoid redundant calculations per frame.
*   **Native Trigonometry:** Deleted the pre-computed trigonometry list and replaced it with native `sin` and `cos` blocks, relying on TurboWarp's optimized math functions.
*   **Dead Code Elimination:** Removed all unused variables and redundant math blocks (such as `+ 0` or `* 1`).
*   **List Lookup Bypassing:** Cached loaded polygon map data into local variables prior to rendering to bypass heavy list lookups.
*   **String Manipulation Trick:** Used the "last string trick" to instantly delete elements from the `2dseg.surface` list, drastically reducing list modification overhead.
*   **Math Restructuring:** Restructured `2dseg.intersect` math to calculate the common denominator first, saving redundant operations.

## 🏗️ Interpreter & Architecture Refactoring
*   **Early Exit Implementation:** Restructured `Glue3d.execute` with the implementation of early exits to prevent unnecessary nested logic execution.
*   **Script Pre-processing:** Added a pre-execution step that strips comments from loaded scripts. Consequently, this removed the need to verify if a line is empty or a comment inside `run_line`, saving cycles per executed line.
*   **Data Transport Cost Reduction:** Removed the inputs from `run_line` by writing the necessary data directly in-place (hardcoded). This avoids the heavy data transport costs between functions.
*   **List Access Optimization:** Replaced `item of list` calls inside `run_line` with a local variable to speed up iteration.
*   **UI Cleanup:** Optimized the Slider component by removing the unused "max" input.

## 🖥️ Rendering & Graphics Pipeline
*   **Shadow Inlining:** Removed the shadow input from `engine.pixel` and inlined the shadow logic directly into the main script, reducing function call overhead during pixel rendering.
*   **Skybox Cleanup:** Removed the `set pixelate effect` and `clear graphic effects` blocks for the sun rendering in `engine.3Dskybox`, streamlining the render pass.
*   **Font Conversion:** Converted the custom "whacky" font from vector to bitmap format to reduce rendering load and file size.

## 📁 Assets & File Size Management
*   **Audio Purge:** Reduced total project size by 10 MB by removing all unused songs from the project assets.

## ✨ New Features
*   **Save/Load System:** Added file import and export functionality, allowing users to save and load their states externally.
*   **Function Addon Module:** Introduced a new function addon module to extend engine capabilities *(Note: currently experimental and known to be glitchy)*.
*   **More Permissive Camera Angle:** Expanded camera angle limits from `-45° to 45°` to `-125° to 125°`.
*   **Custom Grid Snap:** Added a custom grid snapping system. *(Note: Currently works only with 3D models, 2D models, and segments.)*
*   **New Block Category (`Scene`):** A whole new category dedicated to scene management is introduced, featuring 4 new blocks:
    *   **`set loading screen`**: Configures the start and end costume numbers to display for the loading screen.
    *   **`load scene`**: Loads a scene saved in a `scenemap` file type by simply entering the name of the scene to load.
    *   **`start loading screen`**: Starts the loading screen previously configured with `set loading screen`. Can be very useful to hide the visual transition when using `load scene`.
    *   **`stop loading screen`**: Stops the loading screen. Also works if the `start loading screen` block was triggered in another scene.

## ⚠️ Warnings & Release Notes

> [!CAUTION]
> **Loading Screen Configuration**  
> If you use the `start loading screen` block without configuring it beforehand with `set loading screen`, you risk getting weird or unexpected visual results. Always make sure to set up the loading screen configuration before starting it.

> [!WARNING]
> **Script Launch Delay**  
> The *visual* loading of scenes is fast, but the *script* launching might have a slight delay. You may need to add a `wait 0` block after `stop loading screen` to ensure all scripts in the new scene are properly initialized before continuing execution.

> [!NOTE]
> **Camera Angle Limits**  
> The camera angle has been expanded to `-125° to 125°`, but it is not a full 90° symmetrical range. This limit is intentional to prevent visual bugs that occur at extreme angles.

---

*Commit Note: These changes collectively result in a significantly smaller project footprint and much higher frame rates in TurboWarp, though the new function addon module may require further stabilization in subsequent patches. Based on version 1.4 of GoldenYoshi924*
