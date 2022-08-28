phomemo - Command line label printer
====================================

phomemo is a script to quickly print a label from the Linux command line.


Background
----------

I bought a Phomemo M02S label printer to quickly print labels that look better
than my ugly hand-written ones. While the printer itself is great, the software
isn't:

* it's only for IOS and Android - so no fast typing on the keyboard
* it seems to be spyware - why the hell won't it run if it can't access my
	location data?
* it has tons of options that I'm not interested in - images, fancy borders,
	emoji - which just means navigating a lot of menus in addition to
	typing the label text itself.

So, I made this command line script that lets you just type "phomemo labeltext"
and get a label with that text. You can still configure some things like
font and label width (1/3, 2/3, full width).


Installation
------------

You need perl and perl-GD installed. You probably have perl anyway. On Ubuntu,
use `apt install libgd-perl` to get GD; on other linuxes, use your native
package installer. The lib name might be different on those.

Copy the `phomemo` file to anywhere in your path. Copy `phomemo.cfg` to
$HOME/.config/phomemo.cfg`.

`font`: Choose a font to use. The default one needs the `ttf-mscorefonts-installer`
package installed. You can use any TrueType font though, just specify its
file name.

`paperwidth`: this is the number of pixels the printer can print. The M02S
can print 576 pixels; the M02 seems to want 384.

`divisor`: The width of your actual label. Typical values are 1 (full width),
2 (half width), and 3 (1/3 width), which are the sizes of the label stickers
that came with my printer.

`device`: The device name of the printer. On a USB printer, this is typically
`/dev/usb/lp1` unless you have more than one USB printer. The raw serial 
device (`/dev/ttyACM0`, again, unless you have more than one) works as well.
However, you *need* to be in a group that has access to those devices.
Either do `sudo adduser <username> lp` if you want to use the
`/dev/usb/lpX` device, or `adduser <username> dialout` of you want
to use `/dev/usb/ttyACMX`. Remember to relog after that.

Installation with Bluetooth
---------------------------

While I recommend using USB, as you need to keep the printer charged anyway,
you can use Bluetooth as well.  Use hcitool to find out your printer's MAC
address, then rfcomm to provide a serial device for it:

```
# hcitool scan
Scanning ...
	04:7F:0E:56:12:17	M02S
# rfcomm connect 1 04:7F:0E:56:12:17
Connected /dev/rfcomm1 to 04:7F:0E:56:12:17 on channel 1
Press CTRL-C for hangup
```

Now you can use /dev/rfcomm1 to access the printer. You also need to be
in the `dialout` group to do that.

Usage
------

Easy as pie. Type `phomemo "Hello World"` to get a Hello World label.

You can override all of the config file parameters if you want like this:

```
phomemo --font /usr/share/fonts/truetype/dejavu/DejaVuSans.ttf --divisor 2 --device /dev/rfcomm3 "Hello World"
```

where the option names are just the names from the config file.


Other printers?
---------------

Maybe, maybe not. The M02S is the only one I own so I can't tell anything about
other models or brands. The command set seems to be standardized (Epson POS)
so there is a chance to get others to work as well; the main problem
is probably figuring out the correct paper width.
