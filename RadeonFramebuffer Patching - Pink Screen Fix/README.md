# RadeonFramebuffer Patching

For AMD HD 7xxx series or newer 

One of the many features of WhateverGreen is that it supports RadeonFramebuffer patching via device property injection. This can be used to manually inject custom connectors to fix black screen issue or the famous HDMI-DVI connection pink screen issue.

Specifying custom connectors can mainly help with

- Display issues: When automatic connector detection is incompatible and require modifying the connector properties manually (This also requires dumping and decoding vBIOS ROM to gather necessary data)
- Black screen with multiple monitor: Manually setting the connectors priority may fix black screen issue. By default, the order of connectors priority is set to auto(0) by type (Alternatively, connectors priority can also be set with Sense IDs in order via device property injection)

___
This guide will mainly look at the patch for fixing the pink screen behavior on DVI port of RX 580 as an example. (Change DVI connector type to HDMI connector type)

Note: Pink screen issue can also be resolved by using the edid patch script from [here](https://gist.github.com/adaugherity/7435890)

Preparation:

- Lilu.kext (Debug)
- WhateverGreen.kext (Debug)
- Remove framebuffer injection such as Orinoco FB. 
- Boot-arg: -liludbgall liludump=60 (Creates log in /var/log/Lilu_xxx.txt 60 sec after the boot)


Steps:

1. Gather the connector information from the Lilu log (Look for the lines where the AMD GPU's connectors information is being updated)
2. Construct frambuffer code
3. From the constructed framebuffer, modify the DVI connector type to HDMI
4. Update Config.plist (Add array of bytes under the device path of dGPU as a data for "connectors" property)


___

**Gathering the connector information from the log**
<br>

Once you have reached the desktop with the prep settings, open the Lilu log file in /var/log/Lilu_xxx.txt, search for "HDMI" or "DP" and navigate to WhateverGreen log for the connector information.

<img width="861" alt="270129737-2cc47148-2adb-4851-a09e-7e0efc69bfbf" src="https://github.com/AppleBreak1/Sapphire-Rx-580-Nitro-S.E./assets/97265013/ac3fa956-c0a2-466c-b36d-c3042c7534e8">

___

**Framebuffer Code Construction**

Here is what we can collect from the log above

  | Ports | Connector type | Controlflags | Features, Priority | Transmitter, Encoder, Hotplug ID, Sense ID |
  | :-: | :-: | :-: | :-: | :-: |
  | DP | type 00000400 | flag 00000304 |  feat 0100 pri 0000 | txmit 12 enc 04 hotplug 06 sense 01 |
  | DP | type 00000400 | flag 00000304 |  feat 0100 pri 0000 | txmit 22 enc 05 hotplug 04 sense 03 |
  | HDMI | type 00000800 | flag 00000204 |  feat 0100 pri 0000 | txmit 11 enc 02 hotplug 01 sense 02 |
  | HDMI | type 00000800 | flag 00000204 |  feat 0100 pri 0000 | txmit 21 enc 03 hotplug 05 sense 04 |
  | DVI | type 00000004 | flag 00000004 |  feat 0100 pri 0000 | txmit 00 enc 00 hotplug 03 sense 06 |

<br/>

Based on the information provided, we can now construct the code in the order as below
   
      Order: [type] [flag] [feat,pri] [txmit,enc,hotplug,sense]

<br/>

Construct the code for the first connector (DP)

      00040000 [type]
      
      00040000 04030000 [type] [flag]
      
      00040000 04030000 00010000 [type] [flag] [feat,pri]
      
      00040000 04030000 00010000 12040601 [type] [Controlflag] [feat,pri] [txmit,enc,hotplug,sense]
 
  * Note that type, flag, feat, and pri are in reverse byte order

<br/>

Do the same for the remaining connectors. Once done, it should look something like below

      DP:    00040000 04030000 00010000 12040601       
      DP:    00040000 04030000 00010000 22050403        
      HDMI:  00080000 04020000 00010000 11020102        
      HDMI:  00080000 04020000 00010000 21030504        
      DVI:   04000000 04000000 00010000 00000306        


For display connector types, [refer](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/intel-patching/connector.html#patching-connector-types) to Dortania's guide.
___

**Fix for Pink Screen**


Now that FB code is made, we can fix pink screen issue by changing the connector type and controlflag of DVI to HDMI. 

<br/>

Modify the type and flag (In brackets below) of DVI connector
~~~
[04000000 04000000] 00010000 00000306      (DVI)  
~~~

To HDMI
~~~
[00080000 04020000] 00010000 00000306      (HDMI)  
~~~

Now the data to be added in config.plist should look like
~~~
00040000 04030000 00010000 12040601    
00040000 04030000 00010000 22050403    
00080000 04020000 00010000 11020102    
00080000 04020000 00010000 21030504    
00080000 04020000 00010000 00000306   <---- Patched
~~~
___

**Update the Config.plist**

DeviceProperties-> Device path (PciRoot(0x0)...) -> connectors | Data | Patched Framebuffer

<img width="1110" alt="270135119-2d0a623d-c2cf-448d-9679-e7be8f66689a" src="https://github.com/AppleBreak1/Sapphire-Rx-580-Nitro-S.E./assets/97265013/8bad2a22-48df-429c-83a6-4a4e8cc81e24">

___

**Result**

If the patch was successful, Lilu log should look like below (Also contains the result of patching connectors priority)


<img width="865" alt="4" src="https://github.com/AppleBreak1/Sapphire-Rx-580-Nitro-S.E./assets/97265013/8a4c2b1f-7587-4dfe-8210-adbaf44839eb">


Resources

- Acidanthera's [WhateverGreen](https://github.com/acidanthera/WhateverGreen)

- AMD Framebuffer Patching [Guide](https://www.insanelymac.com/forum/topic/303186-how-to-modification-of-amd-fb-clover-injection/)



