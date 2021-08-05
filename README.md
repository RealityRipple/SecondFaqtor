# ![](https://github.com/RealityRipple/SecondFaqtor/raw/master/key.png) SecondFaqtor
Two-Factor authentication on your Linux PC, backed by AES-256 security.

#### Version 1.2.1
> Author: Andrew Sachen  
> Created: February 2, 2020  
> Updated: August 4, 2021

Language: Gambas  
Compiler: Gambas 3.15.0+

##### Involved Technologies:
* OTPAUTH
  * BASE32
  * OTPAUTH URL
* AES (256-bit)
  * OpenSSL
  * Zip file storage using AES-256 (PBKDF2)
    * Optional, non-standard, improved AES-256 ([ZIP-AES-PLUS](https://gist.github.com/RealityRipple/a32f2192501f4775aff36ce143ac6894))
* QR Code Capture and Decoding provided by [ZBar](http://zbar.sourceforge.net/)

## Building
This application can be compiled using Gambas 3.15 or newer.

This application is *not* designed to support Cygwin compilation and may not work on Windows or OS X systems. There may also be internal code which supports Linux Qt UI-drawing methods specifically and may perform poorly or incorrectly on alternate Operating Systems.

## Documentation
SecondFaqtor supports a command-line parameter, which is used when SecondFaqtor is initiated by the `otpauth:` protocol, and a standard `totp`-based URL to import should be passed. SecondFaqtor will display a prompt with information about the imported URL, allowing the user to add it to the list of profiles or ignore it.

Profiles are stored in a JSON file in the Home directory, under the path `~/.config/SecondFaqtor.json`.  
The secret value for each profile is stored in an encrypted format - however, unless the user selects a Password to protect their profiles, the decryption key is also stored in the Config, meaning the security is little more than obfuscation.  
If a password is chosen, the password is run through PBKDF2 with HMAC-SHA-512, a randomized salt, and however many iterations your computer can manage in 1 second, which should be in the tens or hundreds of thousands of rounds.

A "backup" feature adds the functionality of being able to save and restore profiles. This feature relies on the widely accepted Zip and JSON formats.  
Each profile gets a uniquely named file, stored without compression using standard AES-256 encryption as defined by the [Zip AEx Specification](https://www.winzip.com/win/en/aes_info.html), which can be opened by a variety of archiving tools.  
This specification, however, limits the implementation of PBKDF2 by setting a constant iteration value and HMAC algorithm. To improve the security of exported profiles, SecondFaqtor includes the option to use a newly created [addendum](https://gist.github.com/RealityRipple/a32f2192501f4775aff36ce143ac6894) to the specification, as well as some smart code to determine the best number of rounds to use on each device, scaling upward with hardware improvements. Zip files using this altered encryption method can still be accessed and modified by existing archiving software, but the internal JSON files will fail to decrypt.  
For rationale and extra details, please read the above-linked addendum.

## Download
You can grab the latest release from the [Official Web Site](https://realityripple.com/Software/Applications/SecondFactor/For-Linux/).

## License
This is free and unencumbered software released into the public domain, supported by donations, not advertisements. If you find this software useful, [please support it](https://realityripple.com/donate.php?itm=SecondFaqtor)!
