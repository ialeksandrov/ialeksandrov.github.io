---
layout: post
title:  "How to fix sound card not detected - DummyOutput in sound settings after broken firmware upgade: Failed to recover ME Firmware"
date:   2023-10-10 22:21:14 +0300
categories: jekyll update
---
### How to fix sound card not detected - DummyOutput in sound settings after broken firmware upgade: Failed to recover ME Firmware

After a firmware update I've started experiencing a sound issue on lenovo ThinkPad T15 having Ubuntu 22.04. \
There is no sound at all, the sound settings displaying a single "Dummy Output" entry. \
This issue was really annoying and I solved it after a long time Googling. \
\
To Solve this issue open a terminal and execute: \
```$ sudo fwupdmgr reinstall ```

Choose a device:
```0.  Cancel
1.  2292ae5236790b47884e37cf162dcf23bfcd1c60 (Embedded Controller)
2.  349bb341230b1a86e5effe7dfe4337e1590227bd (Intel Management Engine)
3.  04e17fcf7d3de91da49a163ffe4907855c3648be (MZVLB1T0HBLR-000L7)
4.  0d5d05911800242bb1f35287012cdcbd9b381148 (Prometheus)
5.  73e606488fec47b3e3f9288094f66fded0051446 (Prometheus IOTA Config)
6.  a45df35ac0e948ee180fe216a5f703f32dda163f (System Firmware)
7.  362301da643102b9f38477387e2193e57abaa590 (UEFI dbx)
2 <-- picking the second entry \
\# (...) \
 reboot
```

Then During the system reboot, I faced the same errors again:

```
1) "Reading ME Firmware... Please do not power off! 50% Completed"
2) "Recovering ME Firmware...Please do not power off! 10% Completed"
3) "Failed to recover ME Firmware..."
```

But this is normal, at this stage the system is not aware of the existence of the change I asked in the ```fwupdmgr reinstall``` command.

After that it passed by the splashscreen and then the screen stays black for roughly 1-2min.

It finally prints "System Reset" on the top-left.

And it rebooted again. Normally this time.

Everything is working fine now.