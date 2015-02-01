# Interface Alias

You can create interface aliases with `ifconfig`:

    $ sudo ifconfig lo0 alias 127.0.0.2 up

These aliases are *not* persistent, so they need to be made after a restart or login.

This script helps to add a number of local aliases starting from 127.0.0.2.

    $ sudo interface-alias 2 # add 127.0.0.2 and 127.0.0.3 alias to lo0

It is meant to be used with [`launchd`][launchd] to run on startup or login. I used [Lingon][] to
create a property list that `launchd` can use, reproduced here in case you don't have or use Lingon.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.thomasupton.interface-alias</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/interface-alias</string>
        <string>2</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>StandardErrorPath</key>
    <string>/var/log/interface-alias.log</string>
    <key>StandardOutPath</key>
    <string>/var/log/interface-alias.log</string>
</dict>
</plist>
```

Modify the name, paths, and arguments as appropriate - this plist creates two aliases - and place in
`/Library/LaunchDaemons`, then load with `launchctl load /Library/LaunchDaemons/com.thomasupton.interface-alias.plist`.

  [launchd]: https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/launchd.8.html
  [Lingon]: https://www.peterborgapps.com/lingon/
