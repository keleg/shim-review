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
https://github.com/rhboot/shim/tree/13

-------------------------------------------------------------------------------
URL for a repo that contains the exact code which was built to get this binary:
-------------------------------------------------------------------------------
``` no-highlight
The source code used to build shim is not stored in a repo but rather prepared
at build time as follows:

1. Get and unpack https://github.com/rhboot/shim/releases/download/13/shim-13.tar.bz2
2. Apply the following patches:

// From shim v14+
Patch0:        shim-13-MokManager-Stop-using-EFI_VARIABLE_APPEND_WRITE.patch
// From Ubuntu 18.04
Patch1:        shim-13-define-abort-to-avoid-an-unnecessary-reloc.patch

// 2 more upstream patches
Patch2:        shim-13-Cryptlib-replace-CryptPem-with-CryptPemNull.patch
Patch3:        shim-13-CryptLib-Add-the-AsciiStrCpy-decl.patch

The patches and the build instructions (RPM spec file) are here:
https://abf.io/signer/shim-unsigned
```

-------------------------------------------------------------------------------
What patches are being applied and why:
-------------------------------------------------------------------------------
``` no-highlight
* (From shim v14+)
https://abf.io/signer/shim-unsigned/blob/rosa2016.1/shim-13-MokManager-Stop-using-EFI_VARIABLE_APPEND_WRITE.patch
Important bug fix, affects HP laptops: https://github.com/rhboot/shim/issues/105

* (From Ubuntu 18.04)
https://abf.io/signer/shim-unsigned/blob/rosa2016.1/shim-13-define-abort-to-avoid-an-unnecessary-reloc.patch
Build fix.

* (2 more upstream patches, build fixes)
https://abf.io/signer/shim-unsigned/blob/rosa2016.1/shim-13-Cryptlib-replace-CryptPem-with-CryptPemNull.patch
https://abf.io/signer/shim-unsigned/blob/rosa2016.1/shim-13-CryptLib-Add-the-AsciiStrCpy-decl.patch
```

-------------------------------------------------------------------------------
What OS and toolchain must we use to reproduce this build?  Include where to find it, etc.  We're going to try to reproduce your build as close as possible to verify that it's really a build of the source tree you tell us it is, so these need to be fairly thorough. At the very least include the specific versions of gcc, binutils, and gnu-efi which were used, and where to find those binaries.
-------------------------------------------------------------------------------
``` no-highlight
1. OS: ROSA Fresh R10 with all updates from the official repositories
Installation ISO image for i586 builds:
http://mirror.rosalab.ru/rosa/rosa2016.1/iso/ROSA.Fresh.R10/ROSA.FRESH.PLASMA.R10.i586.uefi.iso
MD5: 6affeb3e3ca51b5d323e66e85ca75387

You can install the OS in a VM (VirtualBox or QEMU) using its graphical installer,
the same way as Fedora or Ubuntu.

2. After installation, make sure the network connection is working in the installed OS
and run 'urpmi --auto-update' as root there. This will perform software update and may
take a while depending on how fast the network is (up to 2-3 hours in our experiments).

3. Install build requirements:
urpmi rpm-build git gnu-efi 'pkgconfig(libelf)' openssl 'pkgconfig(openssl)' pesign
(single quotes around pkgconfig() are essential)

No need to install GCC and binutils separately, these will be already installed by this time.

For the record, here are the versions of the components you mentioned:
GCC:        5.5.0_2017.10 (Linaro)
binutils:   2.26.1
gnu-efi:    3.0.8

4. Create the build directories:
$ mkdir -p ~/rpmbuild
$ cd ~/rpmbuild/
$ mkdir -p SOURCES SPECS BUILD BUILDROOT SRPMS RPMS/{noarch,i586,x86_64}

5. Get the sources, patches and the RPM spec file:
$ git clone https://abf.io/signer/shim-unsigned.git -b rosa2016.1
$ cd shim-unsigned
$ cp *.cer *.patch *.rpmlintrc ~/rpmbuild/SOURCES/
$ cp shim-unsigned.spec ~/rpmbuild/SPECS/
$ cd ~/rpmbuild/SOURCES/
$ wget https://github.com/rhboot/shim/releases/download/13/shim-13.tar.bz2

Prepare the source tree in case you want to inspect it:
$ cd ~/rpmbuild/SPECS/
$ rpmbuild -bp shim-unsigned.spec

This will unpack the source tarball and apply the patches mentioned above.
The source tree will be in ~/rpmbuild/BUILD/shim-13/.

Now you can either use 'rpmbuild -bb shim-unsigned.spec' to build shim binary or do it manually:
$ cd ~/rpmbuild/BUILD/shim-13
$ make 'DEFAULT_LOADER=\\\\grubia32.efi' VENDOR_CERT_FILE="${HOME}/rpmbuild/SOURCES/rosa.cer" EFIDIR=rosa all

shimia32.efi should be build on i586
```
-------------------------------------------------------------------------------
Location of certificate
-------------------------------------------------------------------------------
https://github.com/keleg/shim-review/blob/ntcitrosa32/rosa.cer

-------------------------------------------------------------------------------
Microsoft Uefi submission ID
-------------------------------------------------------------------------------
``` no-highlight
13868295317492157
```
-------------------------------------------------------------------------------
Which files in this repo are the logs for your build?   This should include logs for creating the buildroots, applying patches, doing the build, creating the archives, etc.
-------------------------------------------------------------------------------
i586:
Build log: http://file-store.rosalinux.ru/api/v1/file_stores/6599616e1a8d407259424e39eb7fa1790941ac5f.log?show=true
Build info: https://abf.io/build_lists/2953576
Build log (copy on github): https://github.com/keleg/shim-review/blob/ntcitrosa32/shim-ia32-build-2953576.log

-------------------------------------------------------------------------------
Patches to GRUB
-------------------------------------------------------------------------------
In case we need to provide the extra patches to grub we use, they are available
here along with the build instructions (RPM spec file), etc.:
https://abf.io/import/grub2
