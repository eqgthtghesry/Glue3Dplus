# Glue3d+: a more optimized engine with more functionality than any other version of Glue3d.


# Engine Update: Performance Overhaul & Feature Expansion

This update introduces massive performance improvements, structural refactoring, and new features to the game engine. A specific focus was placed on optimizing execution for the **TurboWarp runtime**, reducing overhead in the custom interpreter, and minimizing project file size.

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

---

*Commit Note: These changes collectively result in a significantly smaller project footprint and much higher frame rates in TurboWarp, though the new function addon module may require further stabilization in subsequent patches.*
