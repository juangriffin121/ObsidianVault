---
id: device control
aliases: []
tags:
  - Programming
---

Most hardware devices are controlled by a program by the program writing to a [[Memory Address]] in its [[Virtual Memory]] which instead of mapping to [[RAM]] it maps to the devices inputs like [[Arduino]] pins or stuff like that.
Usually libraries for those things specify which addresses are specifically assigned to the device's inputs.
Also device's output like keyboards, sensors etc are also assigned to addresses which can be read.

- Writing to an address assigned to a device gives that data as input to the device.
- Reading from an address assigned to a device returns to the program the data from that device's output
