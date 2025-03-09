# Lower Power Consumption Reporting by Monitor Apps


Generally, the "PP_EnableLoadFalconSmcFirmware" paramater is set to disabled (0x0) if the framebuffer is not specified and the power consumption reported by monitoring apps for my RX 580 are around 90w~100w on idle. 

To change this behavior, we can override PP_EnableLoadFalconSmcFirmware preference with the help of WhateverGreen via DeviceProperty injection. If successful, the idle power consumption report should be around 30W.

Note: If Orinoco framebuffer is injected, PP_EnableLoadFalconSmcFirmware is set to 0x1 by default.

<img width="1107" alt="3" src="https://github.com/user-attachments/assets/e68ff99d-ec3f-40f4-9f6d-51fe78f8f5ad" />

[WorkLoadPolicyMasks](https://github.com/acidanthera/WhateverGreen/commit/66547393fd2686b55bee00c543630e3a509b4a84#diff-ee8283e40f029316c85e6afc9a45553e903a15fd5ce775633fa26d0d3dbf31b9R83)
