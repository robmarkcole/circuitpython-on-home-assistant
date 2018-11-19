Use ampy to view the boot.py file on the huzzah:
```
Robins-MacBook:board_files robincole$ ampy --port /dev/tty.SLAB_USBtoUART get boot.py

# This file is executed on every boot (including wake-boot from deepsleep)
#import esp
#esp.osdebug(None)
import gc
#import webrepl
#webrepl.start()
gc.collect()
```

We can create and upload a `main.py` file, then view it:
```
Robins-MacBook:board_files robincole$ ampy --port /dev/tty.SLAB_USBtoUART put main.py

Robins-MacBook:board_files robincole$ ampy --port /dev/tty.SLAB_USBtoUART get main.py

print("hello")
```
