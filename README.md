# TSF Minimal MSIX Reproduction

This repository contains a minimal x64 C++ reproduction for a TSF registration issue observed only when the executable has MSIX package identity.

## Environment

- Windows 11 Pro 25H2
- Build 26200.9168
- Architecture: x64

## Results

### Unpackaged

```text
CoInitializeEx=0x00000000
CoCreateInstance=0x00000000
Register=0x00000000
```

### Same executable packaged as minimal MSIX

```text
CoInitializeEx=0x00000000
CoCreateInstance=0x00000000
Register=0x80004005
```

## Package characteristics

- One x64 full-trust executable
- `runFullTrust`
- Fresh CLSID
- No `windows.comServer`
- No `classicAppCompat`
- No IME DLL
- Package: `MAUNGKAUNG.TsfMinimalProbe_1.0.0.0_x64__3exj47kn9m28y`
- SignatureKind: `Developer`
- Status: `Ok`

## Build

```powershell
cmake -S . -B build -A x64
cmake --build build --config Release
```

## Package

Stage these files in an MSIX folder:

- `TsfMinimalProbe.exe`
- `AppxManifest.xml`
- `Assets\StoreLogo.png`
- `Assets\Square44x44Logo.png`
- `Assets\Square150x150Logo.png`

Then run:

```powershell
makeappx pack /d <MSIX_STAGE_FOLDER> /p TsfMinimalProbe-x64.msix /o
```

## Sign

The test package was signed with a local developer certificate matching the manifest publisher.

```powershell
signtool sign /fd SHA256 /sha1 <DEVELOPER_CERT_THUMBPRINT> /s My TsfMinimalProbe-x64.msix
```

## Install

```powershell
Add-AppxPackage -Path .\TsfMinimalProbe-x64.msix
```

## Launch

```powershell
explorer.exe "shell:AppsFolder\MAUNGKAUNG.TsfMinimalProbe_3exj47kn9m28y!Probe"
```

## Log

```text
%LOCALAPPDATA%\MyanglishIME\tsf-minimal-probe.log
```

The same probe succeeds unpackaged but fails with `0x80004005` from `ITfInputProcessorProfiles::Register` when launched with MSIX package identity.
