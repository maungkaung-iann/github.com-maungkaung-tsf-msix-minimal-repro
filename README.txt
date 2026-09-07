TSF Minimal Probe Reproduction

Environment:
Windows 11 Pro 25H2
Build 26200.9168
Architecture: x64

Unpackaged result:
CoInitializeEx = S_OK
CoCreateInstance = S_OK
ITfInputProcessorProfiles::Register = S_OK

Packaged MSIX result:
CoInitializeEx = S_OK
CoCreateInstance = S_OK
ITfInputProcessorProfiles::Register = 0x80004005

BUILD
cmake -S . -B build -A x64
cmake --build build --config Release

PACKAGE
makeappx pack /d <MSIX_STAGE_FOLDER> /p TsfMinimalProbe-x64.msix /o

SIGN
signtool sign /fd SHA256 /sha1 <DEVELOPER_CERT_THUMBPRINT> /s My TsfMinimalProbe-x64.msix

INSTALL
Add-AppxPackage -Path .\TsfMinimalProbe-x64.msix

LAUNCH
explorer.exe "shell:AppsFolder\MAUNGKAUNG.TsfMinimalProbe_3exj47kn9m28y!Probe"

LOG
%LOCALAPPDATA%\MyanglishIME\tsf-minimal-probe.log

Package:
MAUNGKAUNG.TsfMinimalProbe_1.0.0.0_x64__3exj47kn9m28y
SignatureKind: Developer
Status: Ok
