This is for Sapphire Rx 580 Nitro+ Special Edition 8 GB versions.

# OCModeBIOS: 

PowerPlayTable is extracted directly from the Performance mode vBIOS

- Clock: 1430 MHz @ 65288
- Memory: 2100 MHz @1000 mV
- Min Temp: 40c
- Med Temp: 65c
- High Temp: 85c
- Target Temp: 77c
- Zero Fan: On
- Min PWM: 20%
- Med PWM: 40%
- High PWM: 60%

# SilentModeBIOS: 

PowerPlayTable is extracted directly from the Silent mode vBIOS

- Clock: 1340 MHz @ 65288
- Memory: 2000 MHz @1000 mV
- Min Temp: 40c
- Med Temp: 65c
- High Temp: 85c
- Target Temp: 77c
- Zero Fan: On
- Min PWM: 20%
- Med PWM: 40%
- High PWM: 60%

# UnderClocked: 

A modified PowerPlayTable from the original vBIOS.

- Clock: ***1243 MHz @ 65283***
- Memory: ***1750 MHz @ 975 mV***
- Min Temp: ***50c***
- Med Temp: ***65c***
- High Temp: ***85c***
- Target Temp: ***60c***
- Zero Fan: ***OFF***
- Min PWM: ***12%***
- Med PWM: ***25%***
- High PWM: ***40%***

  Please note that even with "Zero Fan" mode off, fans may not kick in until the minimum PWM requirement is reached. For the ones I have, it is 12%, but the other replacements or modded fans may behave differently. You may run Sapphire Trixx app on Windows to find out the minimum PWM. I have modded the powerplay table accordingly so that the fans will constantly run at 12% PWM until the GPU reachs 50c and accelerate then on.

