# how2chroot - basic chroot example

    git clone https://github.com/bmalehorn/how2chroot
    sudo chroot how2chroot /bin/bash
    
Should work on any Linux x86_64 machine, regardless of distro.

## How it was made

    ldd /bin/bash

	linux-vdso.so.1 =>  (0x00007ffff4bc1000)
	libtinfo.so.5 => /lib/x86_64-linux-gnu/libtinfo.so.5 (0x00007f3807e14000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f3807c10000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f3807846000)
	/lib64/ld-linux-x86-64.so.2 (0x000055e2a1b13000)
    
Copy the above libraries, minus `linux-vdso.so.1` (statically linked?).

    mkdir how2chroot
    cd how2chroot
    mkdir -p lib/x86_64-linux-gnu
    mkdir -p lib64
    cp /lib/x86_64-linux-gnu/libtinfo.so.5 lib/x86_64-linux-gnu/
    cp /lib/x86_64-linux-gnu/libdl.so.2 lib/x86_64-linux-gnu/
    cp /lib/x86_64-linux-gnu/libc.so.6 lib/x86_64-linux-gnu/
    cp /lib64/ld-linux-x86-64.so.2 lib64/
    
If symlink, copy destination as well.
    
    readlink \
        /lib/x86_64-linux-gnu/libtinfo.so.5 \
        /lib/x86_64-linux-gnu/libdl.so.2 \
        /lib/x86_64-linux-gnu/libc.so.6 \
        /lib64/ld-linux-x86-64.so.2
    libtinfo.so.5.9
    libdl-2.23.so
    libc-2.23.so
    /lib/x86_64-linux-gnu/ld-2.23.so
    cp /lib/x86_64-linux-gnu/libtinfo.so.5.9 lib/x86_64-linux-gnu/
    cp /lib/x86_64-linux-gnu/libdl-2.23.so lib/x86_64-linux-gnu/
    cp /lib/x86_64-linux-gnu/libc-2.23.so lib/x86_64-linux-gnu/
    cp /lib/x86_64-linux-gnu/ld-2.23.so lib/x86_64-linux-gnu/

Copy `/bin/bash`:

    cp /bin/bash bin/

`chroot` and enjoy!

    sudo chroot how2chroot /bin/bash

Largely copied from
https://www.cyberciti.biz/faq/unix-linux-chroot-command-examples-usage-syntax/
