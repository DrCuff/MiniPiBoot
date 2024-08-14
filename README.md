# MiniPiBoot

Was looking for a way to boot a pi quickly.  tftp is a bit slow, turns out there's now a "Mode 7" to boot a pi with HTTP if you make your own image.  If you want to use https://buildroot.org you can use this as a starter:  https://github.com/raspberrypi/rpi-imager/

Here's what you need to do, starting with a standard [PiOS image, I use the lite ones](https://www.raspberrypi.com/software/operating-systems/)

1. Make a private key:
   ```openssl genrsa -out private.pem 2048```
2. Sign your eeprom:
   ```
      rpi-eeprom-config -c boot.conf -p private.pem -o pieeprom.upd pieeprom.original.bin
      rpi-eeprom-digest -i pieeprom.upd -o pieeprom.sig
   ```
3. Flash it, and reboot so it takes.
```rpi-eeprom-update -f ./pieeprom.upd```

4. Sign your new boot.img you have created with buildroot: ```rpi-eeprom-digest -i boot.img -o boot.sig -k myprivkey.pem```

5. Copy .img and .sig to your webserver.

6. Boot your pi

(video courtesy of ```ffmpeg -i piboot.mov -pix_fmt rgb8 -r 10 output.gif && gifsicle -O3 output.gif -o output.gif```)



