# Finding the process using secure input

When secure input is on, [Karabiner-Elements](https://karabiner-elements.pqrs.org/) stops working. This usually happens when a password prompt appears. It's not always clear which process is asking for the password and blocking Karabiner.

To find out, use this command:

```sh
ioreg -a -l -w 0 | grep -C 3 SecureInputPID
```

This shows the PID using secure input:

```
    // ... output truncated ...
    <key>kCGSSessionSecureInputPID</key>
    <integer>371</integer>
    // ... output truncated ...
```

To identify the process with that PID:

```sh
ps auxww | grep ' 371'
```

Example output:

```
slam  371  0.0  0.3 412659248 103792 ?? Ss 11:24AM 0:17.81 /System/Library/CoreServices/loginwindow.app/Contents/MacOS/loginwindow console
```

In this case, it was the login window. To resolve, try locking and unlocking your Mac. If that doesn't work, you may need to reboot.
