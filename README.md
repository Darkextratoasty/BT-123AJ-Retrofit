# BT-123AJ-Retrofit
This is an old Samsung BT-123AJ TV radio from 1982 with a Raspberry Pi Model 3B+ shoved inside to turn it into a usable standalone computer. 
# Components
* Samsung BT-123AJ TV radio (no idea how available this is, I got it from a family member)
* Raspberry Pi Model 3B+ (Any pi should work)
* [HiFiBerry Zero DAC+](https://www.hifiberry.com/shop/boards/hifiberry-dac-zero/) (or a 4-pole 3.5mm jack)
* [Universal RF modulator for TV](https://www.amazon.com/gp/product/B004IZSXI4/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
* 19V laptop charger (or any 18-20v power supply)
* 5V 2A+ buck converter [(these guys work pretty well)](https://www.amazon.com/Voltage-Regulator-Module-Charger-Converter/dp/B08NJZL2WV/ref=sr_1_21?crid=2HZF8S2SGN03N&keywords=5v+buck+converter&qid=1664129701&qu=eyJxc2MiOiI0LjU1IiwicXNhIjoiNC41NSIsInFzcCI6IjQuNDAifQ%3D%3D&s=electronics&sprefix=5v+buck+converter%2Celectronics%2C129&sr=1-21)
* 12V buck converter (doesn't need much current, but the cleaner the 12V output the better)
* DC barrel plug + jack (or whatever power plug you want to use)

# Modifications
## Raspberry Pi
Since the RF modulator takes in composite video, the pi needs to be set up to output that instead of HDMI. This is actually pretty easy to do, just edit the `/boot/config.txt` file with these changes:
* Comment out `hdmi_force_hotplug=1` to disable hdmi hotplug detection
* Add `hdmi_ignore_hotplug=1` to prevent hotplugging entirely
* Uncomment `sdtv_mode=2` and change it to `sdtv_mode=0` to enable NTSC composite output, you'll need to change modes if your radio isn't from north america.
* Add `sdtv_aspect=1` to set a 4:3 display size
* Add `enable_tvout=1` I think this might only be needed for a pi 4, but it didn't hurt anything to have it in there.

If you're using the HiFiBerry DAC+ Zero, you'll also need to make these changes:
* Add `dtoverlay=hifiberry-dac` to enable the DAC
* Change `dtoverlay=vc4-kms-v3d,composite` to `dtoverlay=vc4-kms-v3d,composite,audio=off` to disable the audio output on the headphone jack
* Create a file called `/etc/asound.conf` and paste the following into it:
	```pcm.!default {
	 type hw card 0
	}
	ctl.!default {
	 type hw card 0
	}
To get the composite video output, I actually didn't have a headphone jack laying around, so I soldered a wire to pad PP24 on the bottom of the board, PP6 right below it is a convenient ground connection nearby. [This guide](https://forums.raspberrypi.com/viewtopic.php?t=89522) was super helpful in figuring out which pads to use.
To get audio out, I soldered wires to the HiFiBerry DAC+ pads labelled L, GND, and R. The only reason I used the DAC hat in the first place is because I didn't have a 4-pole 3.5mm jack laying around, but I did have the DAC hat and hadn't found a use for it in years.
To power the pi, I used a 5V 3A buck converter to step down the 19V input to 5V. Since I didn't have the physical space for a microusb connector in the radio, and the gpio pins were all occupied, I soldered 5V to pad PP2 and GND to pad PP5.
## Universal RF Modulator
For this piece, I basically just removed the housing, 12V transformer, and all the connectors and soldered wires to the board for audio, video, and power input, and TV output. The inputs and outputs were easy, just solder to the points where the connectors had been. For power, my model actually had an unpopulated spot for a DC jack, so I just soldered wires to where that would have been.  Finally the board was hot glued into an open area inside the radio.
## TV Radio
There really wasn't a lot I did to the TV radio, I removed the power supply section, replaced the IO panel, and connected the output of the RF modulator to the VHF input. 
The power section was super easy, it was it's own PCB and a transformer. I simply unplugged it from the main board and tossed it.
The IO panel was a massive stroke of luck, it simply slid out so I didn't even have to cut anything to replace it. I modeled it in Fusion 360 (files above), added some cutouts for the raspberry pi USB and ethernet connectors and a DC jack, and 3D printed a replacement. ***(Need to add the files once I've revised them a bit)*** I also 3D printed a mount for the pi so it just connects to the IO panel and can be slid in and out.
The UHF/VHF input board that used to be connected to the IO panel was tucked back into the radio body and the wires from the TV output of the RF modulator were soldered to the VHF antenna inputs. The UHF antenna inputs were left unpopulated and the switch was hot glued into the VHF position. 
I also cleaned the radio, it was pretty dirty after 20 years of use and another 20 of storage in a basement.

That's it, it was actually a surprisingly easy project. Now I have a raspberry pi 3B+ inside a 40 year old TV radio unit to play with. I do still need to adjust and reprint the IO panel so the pi mount fits better and it's not such a garish color.
# Gallery
### Finished product
![](pictures/finished.jpg?raw=true)
![](pictures/finishedpanel.jpg?raw=true)
![](pictures/screenclose.jpg?raw=true)
![](pictures/audiodemo.mp4?raw=true)
### Internals
![](pictures/internals.jpg?raw=true)
![](pictures/internalss.jpg?raw=true)
### IO Panel
![](pictures/panelopen.jpg?raw=true)
### Raspberry Pi 3B+ with HiFiBerry DAC+ Zero and 5V buck-converter
![](pictures/internalpi.jpg?raw=true)
### RF input board tucked back into radio body
![](pictures/rfinputboard.jpg?raw=true)
