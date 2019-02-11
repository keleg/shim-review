Make sure you have provided the following information:

 - [ ] link to your code branch cloned from rhboot/shim-review in the form user/repo@tag
 - [ ] completed README.md file with the necessary information
 - [ ] shim.efi to be signed
 - [ ] public portion of your certificate embedded in shim (the file passed to VENDOR_CERT_FILE)
 - [ ] any extra patches to shim via your own git tree or as files
 - [ ] any extra patches to grub via your own git tree or as files
 - [ ] build logs


###### What organization or people are asking to have this signed:
`LLC "NTC IT ROSA"`

###### What product or service is this for:
`"ROSA Fresh" - Linux Desktop`

###### What is the origin and full version number of your shim?
`https://github.com/rhboot/shim/tree/13`

###### What's the justification that this really does need to be signed for the whole world to be able to boot it:
`ROSA Fresh is a non-profit Linux distribution developed by the community and has a long history. Is deployed on a high number of nodes already using it in SecureBoot mode enabled.`

###### How do you manage and protect the keys used in your SHIM?
`Shim has the public key of the EV Code Signing key pair (issued by DigiCert) built-in. The key is used to validate GRUB boot loader. No private keys are embedded.Shim binary itself is signed, so the built-in public key cannot be modified or removed without making the signature invalid. This guarantees that if shim has been tampered with and is then used in SecureBoot environments, this will be detected immediately.`

###### Do you use EV certificates as embedded certificates in the SHIM?
`Shim has the public key of the EV Code Signing key pair (issued by DigiCert) built-in.`

###### What is the origin and full version number of your bootloader (GRUB or other)?
`The source code of GRUB is available here: ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz <ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz>. The patches to GRUB 2.02 specific to ROSA Linux, as well as build scripts, are available here: https://abf.io/import/grub2 `

###### If your SHIM launches any other components, please provide further details on what is launched
`Apart from the boot loader (GRUB), shim can launch MokManager tool (developed alongside shim, https://github.com/rhboot/shim )`

###### How do the launched components prevent execution of unauthenticated code?
`Same as GRUB, MokManager executable must be signed with the appropriate key (EV Code Signing) for shim to validate and launch it. MokManager itself executes no unauthenticated code.`

###### Does your SHIM load any loaders that support loading unsigned kernels (e.g. GRUB)?
`Shim launches GRUB`

###### What kernel are you using? Which patches does it includes to enforce Secure Boot?
`Our current kernel is based on the kernel 4.15.0-40.43-generic from Ubuntu 18.04 LTS (http://kernel.ubuntu.com/git/kernel-ppa/mirror/ubuntu-bionic.git/), which already contains the patches to enforce SecureBoot as needed. We have no additional SecureBoot-related patches on top of that.`

`Our patches, configs and build instructions for the kernel (RPM spec file) are available here: https://abf.io/import/kernel-desktop-4.15 `

###### What changes were made since your SHIM was last signed?
```
This is an update from v0.9 to v13. From the changelog:
* MokManager: Stop using EFI_VARIABLE_APPEND_WRITE
* Better PCR usage for TPM
* Use authenticode signature length from WIN_CERTIFICATE structure
* More configurable build via make variables
* Workaround for signtool.exe bugs
* Bug fix for wrong options passed to second stage
* generate_hash(): fix the regression
* Ignore BDS when it tells us we got our own path on the command line
* Handle various different load option implementation differences
* TPM 1 and TPM 2 support`
* Use OpenSSL 1.0.2k
* Lots of minor bug fixes
```

###### What is the hash of your final SHIM binary?
`sha256: 20c2d68f407506861098b4b2921dfb80253996a086ad6e71b31047425b38ffa1 shimx64.efi`
