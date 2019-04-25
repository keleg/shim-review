This repo is for review of requests for signing shim.  To create a request for review:

- clone this repo
- edit the template below
- add the shim.efi to be signed
- add build logs
- commit all of that
- tag it with a tag of the form "myorg-shim-arch-YYYYMMDD"
- push that to github
- file an issue at https://github.com/rhboot/shim-review/issues with a link to your branch

Note that we really only have experience with using grub2 on Linux, so asking
us to endorse anything else for signing is going to require some convincing on
your part.

Here's the template:

-------------------------------------------------------------------------------
What organization or people are asking to have this signed:
-------------------------------------------------------------------------------
``` no-highlight
LLC "NTC IT ROSA"
```

-------------------------------------------------------------------------------
What product or service is this for:
-------------------------------------------------------------------------------
``` no-highlight
"ROSA Fresh" - Linux Desktop
```

-------------------------------------------------------------------------------
What's the justification that this really does need to be signed for the whole world to be able to boot it:
-------------------------------------------------------------------------------
``` no-highlight
"ROSA Fresh" is a non-profit Linux distribution developed by the community
and has a long history. Is deployed on a high number of nodes already using it in SecureBoot mode enabled.
```

-------------------------------------------------------------------------------
Who is the primary contact for security updates, etc.
-------------------------------------------------------------------------------
``` no-highlight
- Name: Gaidukov Andrew
- Position: CTO
- Email address: a.gaidukov@rosalinux.ru
```

-------------------------------------------------------------------------------
Who is the secondary contact for security updates, etc.
-------------------------------------------------------------------------------
``` no-highlight
- Name: Vladimir Potapov (keleg)
- Position: Developer
- Email address: v.potapov@rosalinux.ru
```

-------------------------------------------------------------------------------
What upstream shim tag is this starting from:
-------------------------------------------------------------------------------
Git revision 20e731f4 "VLogError(): Avoid NULL pointer dereferences in (V)Sprint calls".

-------------------------------------------------------------------------------
URL for a repo that contains the exact code which was built to get this binary:
-------------------------------------------------------------------------------
``` no-highlight
https://github.com/rhboot/shim/tree/20e731f4
```

-------------------------------------------------------------------------------
What patches are being applied and why:
-------------------------------------------------------------------------------
``` no-highlight
No additional patches are applied.
```

-------------------------------------------------------------------------------
What OS and toolchain must we use to reproduce this build?  Include where to find it, etc.  We're going to try to reproduce your build as close as possible to verify that it's really a build of the source tree you tell us it is, so these need to be fairly thorough. At the very least include the specific versions of gcc, binutils, and gnu-efi which were used, and where to find those binaries.
-------------------------------------------------------------------------------
``` no-highlight
OS: ROSA Fresh R11 with all updates from the official repositories

1. Here is a disk image of a QEMU VM with the prepared build environment (approx. 4 Gb in size):

rosa-build32.qcow2:
https://drive.google.com/file/d/1sMibJzZZsrHOQUB4N_iF7PAl_VXTmacs/view?usp=sharing
sha1: 334545258cd53c7b1bf160999c8511951655ee20  rosa-build32.qcow2

One can launch the VM as follows:

$ qemu-kvm -name "ROSA-Build32" -cpu host -machine q35 -m 2G -smp 2 -netdev user,id=network0,net=192.168.30.0/24,host=192.168.30.1,hostfwd=tcp::7031-:22 -device e1000,netdev=network0,mac=02:C0:2C:33:B4:3A -drive file=rosa-build32.qcow2,if=virtio,index=0,media=disk

user: builder
pass: rosabuild
root pass: rosabuild

SSH access: ssh -p 7031 builder@localhost

2. To build shim, do the following in the guest OS:

$ cd /home/builder/shim/shim.git
$ git checkout rosa-r11-shim-15-20e731f4
$ make 'DEFAULT_LOADER=\\\\grubia32.efi' VENDOR_CERT_FILE=/home/builder/shim/rosa.cer EFIDIR=rosa all

3. For the record, here are the versions of the components you mentioned:
GCC:        5.5.0_2017.10 (Linaro)
binutils:   2.26.1
gnu-efi:    3.0.9
```
-------------------------------------------------------------------------------
Location of certificate
-------------------------------------------------------------------------------
https://github.com/keleg/shim-review/blob/ntcitrosa32/rosa.cer

-------------------------------------------------------------------------------
Microsoft Uefi submission ID
-------------------------------------------------------------------------------
``` no-highlight
No ID yet, because we are required to pass the review first.
```
-------------------------------------------------------------------------------
Which files in this repo are the logs for your build?   This should include logs for creating the buildroots, applying patches, doing the build, creating the archives, etc.
-------------------------------------------------------------------------------
ia32:
Build info (the build system will keep it till May 20 or so): https://abf.io/build_lists/3006835

Build log (copy on github): https://github.com/keleg/shim-review/blob/ntcitrosa32/build-shim-ia32-3006835.log

-------------------------------------------------------------------------------
Patches to GRUB
-------------------------------------------------------------------------------
In case we need to provide the extra patches to grub we use, they are available
here along with the build instructions (RPM spec file), etc.:
https://abf.io/import/grub2
