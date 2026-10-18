# VulkanDiag

VulkanDiag is a cross-platform compatibility check for Avalonia, SharpEngine, and the GPU. Download the package for the computer you want to check and run it there. The downloadable packages are published in the public vulkan-compatibility repository.

VulkanDiag inventories the installed graphics driver and launches the SharpEngine/Avalonia Vulkan path in isolated child processes. It first tries direct `SharedTexture`, then `SharedTexture` with a custom shared image, then `WriteableBitmap`, and keeps the first mode whose on-screen image matches the GPU readback. Horizontal bands on screen mean the presentation failed, including when the GPU readback itself is valid. The selected mode is then tested at SSAA 1, SSAA 2, and SSAA 4 in fresh processes. The report names that presentation mode.

The verdict answers whether this computer can display an Avalonia and SharpEngine scene. SharpEngine can display the scene when the picture on screen matches the GPU readback. SharpEngine cannot display the scene when that picture fails. GPU compute and hardware ray query are reported separately. A missing compute device moves those operations to the CPU. A missing hardware ray query leaves GPU work on the compute shader and the CPU. SSAA 1, SSAA 2, and SSAA 4 show how far that displayed scene can go. A higher quality that does not render stays on the report, and a lower quality that did render still means SharpEngine can display the scene.

When the picture on screen does not match the GPU readback, VulkanDiag writes the two frames as BMP files and names them in the failure. The isolated probe directory is temporary, so the pictures are kept outside it:

- Windows: `%LocalAppData%\TommyBilt\VulkanDiag`
- Linux: `~/.local/share/TommyBilt/VulkanDiag`
- Mac: `~/Library/Application Support/TommyBilt/VulkanDiag`

VulkanDiag performs no benchmark, sends no telemetry, and does not install or change drivers. When the test fails, it links to the official [Vulkan GPU driver resources](https://www.vulkan.org/tools#vulkan-gpu-resources).

The graphics support policy and the distinction between GPU capability and an actually working installed driver are documented in [TommyBilt graphics requirements](docs/Graphics-Requirements.md).

## Download

Each download is one zip. `SHA256SUMS.txt` is published beside the packages. These builds are unsigned.

**Windows x64.** `VulkanDiag-1.0.5-windows-x64.zip` contains the self-contained executable `VulkanDiag-1.0.5-windows-x64.exe`. Unzip it and run the executable. Windows may ask you to confirm an unsigned program.

**Linux x64.** `VulkanDiag-1.0.5-linux-x64.zip` contains `VulkanDiag-1.0.5-linux-x86_64.AppImage` for mainstream glibc-based x86-64 desktops. After extracting, allow the file to run as a program, or run `chmod +x VulkanDiag-1.0.5-linux-x86_64.AppImage`, then launch it.

**Apple Silicon.** `VulkanDiag-1.0.5-osx-arm64.zip` contains `VulkanDiag.app` for macOS 26 or later. The app includes the Vulkan loader and KosmicKrisp. After extracting, Control-click the app and choose Open, then Open again. If macOS reports that the app is damaged or will not run, move it to Applications and run:

```bash
xattr -cr "/Applications/VulkanDiag.app"
chmod +x "/Applications/VulkanDiag.app/Contents/MacOS/VulkanDiag"
```
