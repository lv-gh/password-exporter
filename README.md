# password-exporter
Fixed version of [Password Exporter](https://addons.thunderbird.net/en-us/firefox/addon/password-exporter/) Mozilla add-on; 
works with SeaMonkey, correctly exports UTF-8 data.

Tested with SeaMonkey 2.49.* (Windows) . 

Add-on commands won't be shown/added to SeaMonkey menu, though GUI is accessible 
through add-on options:
[Menu] **Tools** > **Add-ons Manager** > **Password Exporter** > **Options**

Exported data is UTF-8. 

## Note for KeePass Windows users:

[KeePass](https://keepass.info/) assumes that imported data is of 
[Encoding.Default](https://learn.microsoft.com/en-us/dotnet/api/system.text.encoding.default?view=net-7.0) 
(.NET Framework). On the Windows desktop, the *Encoding.Default* property _always gets the system's 
active code page_, __which is usually ANSI code page__, not UTF-8. 

So, __if exported data contains non-ascii chars__, then before importing it to KeePass:

* It should be converted back from UTF-8 encoding to *Encoding.Default* (if it's possible), e.g.:<br>
`$ iconv -f utf8 -t cp1257 password-exporter-utf8.xml > password-exporter-cp1257.xml`

    Problem: e.g. if password's data (internally stored as Unicode and exported as UTF-8) contains Greek and
Latin accented chars and the system's active code page is 437 (en-US), then it's not possible to import that
data correctly to KeePass because it's not possible to convert it to en-US encoding, so:

**OR**

* [Set a process code page to UTF-8](https://learn.microsoft.com/en-us/windows/apps/design/globalizing/use-utf8-code-page)
(since Windows Version 1903;  May 2019 Update) 

   **Windows Settings** > **Time & language** > **Language & region** > **Administrative language settings** > 
**Change system locale**, and check **"Beta: Use Unicode UTF-8 for worldwide language support"**. 
Then reboot the PC for the change to take effect.

  In this case KeePass assumes that imported data is of UTF-8 (*Encoding.Default*) encoding, which it actually is.
