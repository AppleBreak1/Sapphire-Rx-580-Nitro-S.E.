# Pairing RX580 with Old Processors


Applicable Processors


- The Intel CPUs that support SSE4.1 but lack SSE4.2 support 

Requirements

- macOS: Sierra or newer (Sierra does not have requirement for SSE4.2 for AMD Drivers)
- [Mousse](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/page-8): Required for High Sierra+ (Partial SSE4.2 emulator)

    Note 1: Without the SSE4.2 support, AMD metal drivers do not load on High Sierra+ (Black Screen)<br>
    Note 2: The [patched](https://github.com/dortania/OpenCore-Legacy-Patcher/tree/main/payloads/Kexts/SSE) version of MouSSE by OCLP developers is required for Ventura+

___
Fixing DRM and Hardware Encoding/Decoding

- Big Sur+ 

| SMBIOS 	| GVA Patches| Safari 	| TV+ | Apple Music | H264 Enc | H265 Enc |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  iMac10,1 | unfairgva=7 | ❌ | ✅ | ✅ | ✅ | ❌ |
|  iMacPro1,1 | unfairgva=1 | ✅ | ✅ | ✅ | ✅ | ✅ |


- Mojave

| SMBIOS 	| GVA Patches| Safari 	| iTunes | H264 Enc | H265 Enc |
| :---: | :---: | :---: | :---: | :---: | :---: |
|  iMac10,1 | shikigva=144<br> shiki-id=Mac-7BA5B2D9E42DDD94 | ❌ | ✅ | ❌ | ❌ |
|  iMac10,1 | shikigva=160<br> shiki-id=Mac-7BA5B2D9E42DDD94 | ❌ | ❌ | ✅ | ❌ |
|  iMacPro1,1 | shikigva=80 | ❌ | ✅ | ✅ | ✅ |

- Catalina
   
| SMBIOS 	| GVA Patches | Safari 	| TV+ | Apple Music | H264 Enc | H265 Enc |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  iMac10,1 | shikigva=144<br> shiki-id=Mac-7BA5B2D9E42DDD94 | ❌ | ✅ | ✅ | ❌ | ❌ |
|  iMac10,1 | shikigva=160<br> shiki-id=Mac-7BA5B2D9E42DDD94 | ❌ | ❌ | ❌ | ✅ | ❌ |
|  iMacPro1,1 | shikigva=80 | ✅ | ✅ | ✅ | ✅ | ✅ |


Note 1: For H265(HEVC) encoding capability, switching to iMacPro1,1 is enough<br>
Note 2: ShikiGVA patches are disabled for Big Sur+ and is replaced by unfairgva.
# Property Injection

<img width="686" alt="1" src="https://github.com/user-attachments/assets/53103a05-5fce-49e2-ad66-de7d2baeb713" />

# Connectivity Issues

When booting in legacy mode, following issues are present

- HDMI -> black screen on boot
- DVI to DVI -> black screen on boot
- DVI to HDMI -> pink screen on boot (also present on UEFI mode)

  Workaround: Use DP ports or when having a multiple monitor setup, have the system go through sleep/wake cycle to get screens on HDMI or/and DVI ports.
  
# Resource

- [ShikiGVAPatches](https://github.com/acidanthera/WhateverGreen/blob/bc1b7c334eefd00a9cf30429d686764c18ed59ae/WhateverGreen/kern_shiki.hpp#L35) 
- [DRM Compatibility Chart](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Chart.md)

# Credit
[Syncretic](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/) for developing MouSSE



