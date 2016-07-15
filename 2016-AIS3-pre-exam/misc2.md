# misc2
Auther: HaopingKu  
Date: July 15, 2016

We get a file `misc2` in this game, using `file` command we get only "data".
Open it by `xxd`, we see that the file begins with `7Z...`. It looks like a 7z file, but actually the real 7z file begins with `7z` (downcase 'z'). Use and hex editor wo convert 'Z' to 'z', we can extract files in it.  
Files extracted are another 7z file, and "secret.txt" which has password in it. Use this password, we can extract 7z file again (notice to convert 'Z' to 'z' again).  
Finally, I wrote a script to do this automatically, because the flag is zipped for 1000 times, and finally flag get!!

