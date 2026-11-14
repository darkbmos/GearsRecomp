# GearsRecomp

Port no oficial de **Gears of War** (Xbox 360) a **Android** y **PC** por recompilación estática: el código
PowerPC del juego se traduce a C++ y se compila para ARM64 y x64, con un renderer Vulkan nativo. No es un
emulador. Port de **Jauidev**, hecho con la herramienta de este repositorio que lleva un juego de Xbox 360 del
disco a un APK.

> **No incluye el juego.** Necesitas tu propia copia del disco de Gears of War para Xbox 360.
> Gears of War y sus marcas pertenecen a Microsoft. Este proyecto no está afiliado a Microsoft ni a Epic Games.

## Estado

- Arranca y se juega a **30 fps** (el límite del propio juego) en un POCO X6 Pro (Mali-G615).
- La compatibilidad con SD 8 Gen 3 (Adreno 750) (MEIZU 21) y los drivers turnip funciona bien con ligeras caídas de FPS.
- Desde la 1.1.0 usa un ~17 % menos de CPU que la 1.0.1 y traduce en segundo plano los shaders que faltan
  en la caché, en vez de parar el juego para hacerlo.
- ~95 % del tiempo de CPU es código nativo; lo emulado es el procesador de comandos de la GPU y el audio XMA.
- Controles táctiles y mandos físicos por Bluetooth o USB (Xbox, PlayStation y la mayoría de mandos
  Android), a la vez.
- Probado solo unos minutos de campaña y solo en una GPU Mali.
- Campaña completada hasta la mitad en una GPU Adreno.


## Instalar en Android

1. Descarga la APK de [Releases](../../releases) (arm64, Android 10 o superior, Vulkan).
2. Instálala y ábrela.
3. En el launcher elige la ISO del juego o la carpeta del disco extraído. También puedes copiar el disco
   extraído a `/sdcard/Android/data/com.xport.gears_of_war/files/game`.

En Adreno, el launcher puede usar Mesa Turnip si el driver del sistema no sirve.

## Compilar

Requisitos (Windows): Android NDK 27.2, Android SDK (build-tools 36, CMake 3.31), MSVC Build Tools 2022,
Python 3 y Git Bash. Todo el código de terceros va incluido en `third_party/`.

Del disco al APK, con todos los pasos (recompilar, ranura, shaders, PC, prueba de arranque, APK):

    py -3 -I tools/port.py <juego.iso|carpeta del disco> <carpeta de trabajo> gears_of_war --label "Gears of War"

Con la ranura `games/gears_of_war` ya preparada (como en este repositorio):

    XPORT_GAME=gears_of_war bash tools/build-android.sh xportapp
    XPORT_GAME=gears_of_war bash tools/build-apk.sh

Para PC (PowerShell): `$env:XPORT_GAME='gears_of_war'; cmd /c tools\build-host.bat`.

Los shaders que no están en la caché se traducen en tiempo real; en Android hace falta `libdxcompiler.so`
(`tools\build-dxc-android.bat`).

La guía completa está en [docs/GUIA.md](docs/GUIA.md) y la descripción de las herramientas en
[docs/XPORT.md](docs/XPORT.md).

## Estructura

| Carpeta | Qué hay |
| --- | --- |
| `runtime/` | Runtime: kernel, CPU, audio, launcher |
| `rexglue_glue/`, `xenia_glue/` | Hooks de Direct3D y puente con el renderer |
| `third_party/rexglue/` | ReXGlue SDK y el renderer nativo xnative (de Liberty Recompiled) |
| `games/gears_of_war/` | La ranura del juego: configuración, hooks de D3D, código recompilado (`ppc/`) y shaders |
| `tools/` | XPort: del disco al APK (`port.py`) y utilidades |
| `android/`, `LAUNCHER/` | La app de Android y los recursos del launcher |

## Licencia

[GPL-3.0](LICENSE): el renderer viene de Liberty Recompiled (GPL-3). Cada componente de `third_party/`
conserva su propia licencia.

## Créditos

Port Android: **Jauidev**.

Liberty Recompiled (renderer nativo xnative), ReXGlue SDK, XenonRecomp, XenosRecomp y Unleashed Recompiled
(hedge-dev), Xenia Canary, DirectX Shader Compiler, glslang, smol-v, volk, SMAA, Mesa Turnip, libadrenotools,
libGPUCounters, FFmpeg, libmspack, Zstandard, Snappy, xxHash, unordered_dense, O(1) Heap, spdlog, fmt,
Dear ImGui, stb_image, Khronos Validation Layers y GFXReconstruct.
