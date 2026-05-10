# Repository Guidelines

## Project Structure & Module Organization

This repository contains a Saleae Logic 2 Manchester analyzer plugin. Core C++ sources live in `src/`, with matching `.h` and `.cpp` files for the analyzer, settings, results, and simulation data generator. `CMakeLists.txt` defines the `manchester_analyzer` plugin target and lists all source files explicitly. `cmake/ExternalAnalyzerSDK.cmake` fetches and links Saleae's Analyzer SDK. CI configuration is in `.github/workflows/build.yml`; generated build output belongs under `build/`, which is ignored.

## Build, Test, and Development Commands

- `cmake -B build -DCMAKE_BUILD_TYPE=Release`: configure a local release build on Linux or macOS.
- `cmake --build build`: compile the analyzer and place the plugin under `build/Analyzers/`.
- `cmake -B build -A x64`: configure a Windows Visual Studio x64 build.
- `cmake --build build --config Release`: build the Windows release DLL.
- `clang-format -i src/*.cpp src/*.h`: format source files using the repository `.clang-format`.

The first configure step downloads the Analyzer SDK with CMake `FetchContent`, so network access is required on a clean build.

## Coding Style & Naming Conventions

Use C++11 and follow `.clang-format`: 4-space indentation, Allman braces, no tabs, 140-column limit, left-aligned pointers, and no include sorting. Keep class names in `PascalCase` (`ManchesterAnalyzerResults`), member variables prefixed with `m` (`mBitRate`), and enum constants in the existing uppercase style (`MANCHESTER`, `TOL25`). Keep SDK-facing exported functions and virtual method names consistent with Saleae Analyzer SDK conventions.

## Testing Guidelines

There is currently no dedicated unit test directory or CTest target. Before submitting changes, at minimum run a clean CMake configure and build for your platform. For decoder behavior changes, validate manually in Saleae Logic 2 with representative Manchester, differential Manchester, bi-phase mark, and bi-phase space captures. If adding tests later, prefer a `tests/` directory and wire it through CMake/CTest.

## Commit & Pull Request Guidelines

Recent history uses short, imperative, lowercase commit messages such as `updating readme` and `fixing the github action file`. Keep commits focused and describe the concrete change. Pull requests should include a brief summary, affected analyzer modes or settings, build commands run, and any manual Logic 2 validation performed. Link related issues when available and include screenshots only for UI or decode-output changes.

## Agent-Specific Instructions

Do not commit generated `build/` artifacts. When adding or renaming source files, update the explicit `SOURCES` list in `CMakeLists.txt` and confirm CI artifact paths still match the generated analyzer outputs.
