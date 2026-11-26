# Cross-Platform Graphics & Web Deployment (Android / OpenGL ES → WebGL)

**Specific skill: cross-platform C++ builds and CI/CD** · DigiPen coursework, 2026 · C++, OpenGL ES 3.0, Emscripten, GitHub Actions

**▶ Live demos (no sign-in):** https://testwhale.github.io/android-webgl-showcase/

## Overview

Three C++ OpenGL ES 3.0 applications, first written for Android, that I made build from one codebase for Android, Windows desktop and the browser. Every push to `main` builds them in GitHub Actions and deploys the WebGL builds to GitHub Pages.

## My role and contributions

- The scene code for each assignment. Assignment 3 adds Phong lighting, a procedural checkerboard ground, a 2D HUD drawn over the 3D scene, 3D picking, eased camera and speed, and press-and-hold repeat.
- Emscripten build scripts (`build_web.sh` / `build_web.bat`) for each assignment.
- Three GitHub Actions workflows (Android, Desktop, GitHub Pages) and the Pages landing page.

Course-provided framework files (`Transform`, `main.cpp`, the JNI bridge) are marked as unmodified in each assignment's README.

## Problem

Android OpenGL ES coursework normally runs only on a phone or an emulator, so a reviewer can't simply open it. I wanted the same C++ code to run in any browser, and to be rebuilt and redeployed automatically so the live link always matches the source.

## Constraint

- **One codebase, three targets:** Android enters through a JNI bridge, desktop and web through GLFW, and the scene code is shared.
- **API mapping:** OpenGL ES 3.0 calls have to map onto WebGL 2 in the browser.
- **CI without local tools:** there is no Android Studio, Visual Studio or Emscripten on the build machine; the runners have to set everything up from scratch.

## What I Tested

- The **Android**, **Desktop** (Ubuntu + Windows) and **GitHub Pages** workflows on every push. There have been 39 workflow runs in total.
- All three live demos and the landing page in the browser on 26 Nov 2026. Each one loads, creates a WebGL 2.0 (OpenGL ES 3.0) context and renders, with no console errors.

## What Failed

- **The early pipeline:** 8 workflow runs failed on 3 July 2026 while the Pages build was being set up.
- **Android CI used the wrong Gradle version.** `gradlew` is not committed, so the `setup-gradle` action silently fell back to the runner's Gradle 9.x, which is incompatible with AGP 8.x.
- **Windows CI couldn't find Visual Studio.** The Visual Studio 17 2022 CMake generator intermittently failed on `windows-latest` runners with "could not find any instance of Visual Studio".
- **The live link broke when the repository was renamed** from `zachary-android-assignments` to `android-webgl-showcase`. GitHub Pages does not redirect after a rename, so the old URL stopped resolving.

## What I Changed

- Compiled with `emcc -s USE_WEBGL2=1 -s FULL_ES3=1 -s USE_GLFW=3`, and packed the GLSL shaders into Emscripten's virtual filesystem with `--preload-file`, so the Android shader assets load unchanged.
- Pinned Emscripten to 3.1.64 in the Pages workflow.
- Made the Android workflow read the pinned Gradle version from `gradle-wrapper.properties` and pass it explicitly.
- Switched the Windows build to Ninja with `msvc-dev-cmd`, which avoids the Visual Studio instance lookup.
- Updated the README and this portfolio to the new Pages URL, and corrected the landing page's Assignment 1 description.

## What Happened

- **All three demos run live in the browser:** [Assignment 1](https://testwhale.github.io/android-webgl-showcase/Assignment-1/) (shader primitives), [Assignment 2](https://testwhale.github.io/android-webgl-showcase/Assignment-2-3DFan/) (3D fan with a matrix-stack hierarchy), [Assignment 3](https://testwhale.github.io/android-webgl-showcase/Assignment-3/) (fan with Phong lighting, HUD and picking).
- Every workflow run since 3 July 2026 has passed. The commit checked in this case study (`69c20b8`) passed the Pages, Desktop and Android workflows.

## Technologies

C++17, OpenGL ES 3.0, WebGL 2, GLSL, GLM, GLFW, JNI, Android NDK, Gradle, CMake, Ninja, Emscripten 3.1.64, GitHub Actions, GitHub Pages

## Supporting evidence

- [Live demos](https://testwhale.github.io/android-webgl-showcase/)
- [Repository and README](https://github.com/testWhale/android-webgl-showcase)
- [GitHub Actions run history](https://github.com/testWhale/android-webgl-showcase/actions)

## Source availability

The repository [testWhale/android-webgl-showcase](https://github.com/testWhale/android-webgl-showcase) is public.

[← Back to portfolio](../README.md)
