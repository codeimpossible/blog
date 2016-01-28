---
layout: post
status: publish
title: 'Converting a Security Identifier from binary to string'
slug: "Converting-a-Security-Identifier-from-binary-to-string"
---
Just in case you ever want to convert a binary representation of a Windows Security Identifier (SID) to it's friendlier string version:

Imagine we have a binary sid that is:`0x010500000000000515000000F2EBB9149329116C5E3528360E040000`

That doesn't look too inviting does it? Lets break it out into segments for easier reading.

    0. 0x01
    1. 05 
    2. 000000000005
    3. 15000000
    4. F2EBB914 
    5. 9329116C
    6. 5E352836 
    7. 0E040000

Segments 0 through 2 are used by Windows to validate the SID. 0 is the version number of the SID; currently, all Windows operating systems use "1" as the version. 1 tells us that there are 7 segments in this SID (number of dashes minus 2). Segment 2 states that this sid has a top level authority of 5 which equals **SECURITY_NT_AUTHORITY**. (for a better description of what each security authority does please see [this KB article][1] )

Segments 3 through 6 are little-endian 32-bit integers that describe the issuer of the SID. 7 is a 32-bit little-endian integer called the "Relative ID". This value is the id of the user on the machine that issued the SID. It is important to remember that these values are stored as little-endians because this will factor in when we translate them into the final SID.

the first part of the sid is the easiest to translate. In our example sid above the first 3 segments become "S-1-5" because every sid is prefixed with "S" and we drop segment 3 as it is only used in validation. The next part is where we translate the machine id and user id and this can be confusing when done for the first time. Since the values are stored in little-endian format we have to reverse the order of the bits so:

 - `15000000` becomes `00000015` which translates (hex to dec) to `21`
 - `F2EBB914` becomes `14b9eb2f` which translates (hex to dec) to `347728882`
 - `9329116C` becomes `6c112993` which translates (hex to dec) to `1813064083`
 - `5E352836` becomes `3628355E` which translates (hex to dec) to `908604766`


Now we have our machine identifier and our SID is now `S-1-5-21-347728882-1813064083-908604766`

All we have to do now is translate the user id which follows the same process as the machine id so:

 - **0E040000** becomes 0000040e which translates (hex to dec) to 1038


So we are all done and our final sid is:

    S-1-5-21-347728882-1813064083-908604766-1038

  [1]: http://msdn2.microsoft.com/en-us/library/aa379649(VS.85).aspx
