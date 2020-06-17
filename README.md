# i2c-buses
Not actual code, just a compilation of information about I2C buses on modern PCs: how to use them, what's on them.

## History
I had a broken monitor. I mean, it worked, but Debian wasn't autodetecting it's resolution correctly, so it came up at a failsafe resolution. In the kernel logs, I saw "bad EDID" and started looking into it. I have a second identical monitor that worked fine. In looking to see if I could copy the good EDID to the bad monitor, I learned that there's 10 I2C buses on my computer, with a total of 224 devices hanging off of them. So far, I know what *ONE* of them does, so I decided to learn more, and document it. This is what I've got so far.
## I2C tools
Under Linux, please make sure you modprobe the i2c-dev module so that you can access i2c devices.
### i2cdetect
i2cdetect will scan buses. "i2cdetect -l" will tell you what buses it can see. "i2cdetect -y <bus-number>" will scan a bus and tell you which addresses have a device on them.

### i2cdump
  you can get data from a device using i2cdump. For example, on my system, "i2cdump -y 10 0x50" will get the EDID info from my monitor. In the command, "10" is the bus number, and "0x50" is the device number.
  
### i2cset
  You might be able to use this command to modify data on a device on an I2C bus. Use at your own risk!!!
##Monitors
Monitors report Manufacturer, model, resolution and timing using an I2C bus. The block of info is in a format called EDID. You can find monitors by scanning I2C buses for something at address 0x50, and doing a dump of that. The EDID header starts with 00 FF FF FF FF FF FF FF 00. There's a convenient decoder of the hex data at http://www.edidreader.com/ you can use. There's also something in the i2C tools, but I have yet to figure out how to use it.

## RAM
Modern RAM is in a 168-pin DIMM which includes 2 pins for an I2C bus. You can query the device to get manufacturer, size, and timing information from your ram boards. You can find memory by scanning buses for something at address 0x50, 0x51, 0x52, and 0x53 (one per slot. If your motherboard has more slots, there'll be more addresses.) you can redirect the output of i2cdump to decode-dimms to see what's in there.

## TBD
My motherboard seems to have 4 I2C buses on it, but has only 13 devices, all on the first bus. So far, I can only identify 4 as my memory ( 0x50 - 0x53 ).
My video card has another 4 buses, but with devices only on the last bus, and only 9 of them, at that. So far, I know that 0x50 is to communicate with my monitor.I can atttach up to 4 monitors, so I'm presuming that 0x51 - 0x53 are the same.
There are 3 more buses that i2cdetect labels "drdm". There are 92 devices on each of these buses. 
