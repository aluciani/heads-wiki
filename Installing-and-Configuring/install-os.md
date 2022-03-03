---
layout: default
title: Step 4 - Installing Qubes and other OSes
permalink: /InstallingOS/
nav_order: 8
parent: Installing and configuring
---

<!-- markdownlint-disable MD033 -->
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>
<!-- markdownlint-enable MD033 -->

Generic OS Installation
===

1. Insert OS installation media into one of the USB3 ports (blue on Thinkpads).
[For certian OSes](https://github.com/osresearch/heads/tree/master/initrd/etc/distro/keys)
 , the Heads boot process supports standard OS bootable media (where the USB
 drive contains the installation media which as created using `dd` or
 `unetbootin` etc.) as well as booting directly from verified ISOs on a plain
 old partition.  For example, if the USB drive has a single partition, you can
 put the ISO image along with a trusted signature in the root directory:

```shell
/Qubes-R4.0-x86_64.iso
/Qubes-R4.0-x86_64.iso.asc
/tails-amd64-3.7.iso
/tails-amd64-3.7.iso.sig
```

Each ISO is verified before booting so that you can be sure Live distros and
 installation media are not tampered with or corrupted, so this route is preferred when
 available.  You can also sign the ISO with your own key from Heads recovery shell 
 menu option :

```shell
gpg --output <iso_name>.sig --detach-sig <iso_name>
```

Some distros require additional options to boot properly directly from ISO.  See
 [Boot config files](/BootOptions) for more information.
2. Boot from USB by Boot menu options, or by calling `usb-scan` from the recovery shell.
3. Select the install boot option for your distro of choice and work through the
 standard OS installation procedures (including setting up LUKS disk encryption
 if desired)
4. Reboot and your new boot option should be available through boot options, show boot options.

If you want to set a default option so that you don't have to choose at every
 boot, you can do so from the menu by selecting 'd' on the confirmation screen.
 You will also be able to seal your Disk Unlock Key using into the TPM, allowing
 you to use ensure only the good TPM disk encryption key passphrase and the proper PCR state will unseal 
 the disk unlock key on default boot option selection.

(\*) Ubuntu/Debian Note: These systems don't read `/etc/crypttab` in their
 initrd, so you need to adjust the crypttab in the OS and `update-initramfs -u`
 to have it attempt to use the injected key.  Due to oddities in the cryptroot
 hooks, you also need keyscript to be in `/etc/crypttab` even as a no-op
 `/bin/cat`:

`sda5_crypt UUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX /secret.key luks,keyscript=/bin/cat`

(Credit to [https://www.pavelkogan.com/2015/01/25/linux-mint-encryption/](https://www.pavelkogan.com/2015/01/25/linux-mint-encryption/)
 for this trick).

Installing Qubes 4.1
===
Qubes OS and Tails can boot directly from ISO when provided with accompanying detached signatures (iso.asc or iso.sig), thanks to 
distribution signing keys being provided under Heads, permitting to validate both integrity and authenticity of the ISOs prior of booting into them.

Plug in the ext3/ext4 formatted USB stick containing Qubes R4.1 iso and iso.asc files into one of the USB port and
 boot it from USB mode:

![1-Heads-Options](https://user-images.githubusercontent.com/827570/156627927-7239a936-e7b1-4ffb-9329-1c422dc70266.jpeg)
![2-Heads-Boot-Options](https://user-images.githubusercontent.com/827570/156627934-8051cd38-ad5e-452d-b340-9d13317f33b8.jpeg)
![3-Heads-USB-Boot-Option](https://user-images.githubusercontent.com/827570/156627936-8fd43c94-fd2f-448b-a84c-eadd9166434e.jpeg)
![4-Heads-USB-Boot-Options-ISOs](https://user-images.githubusercontent.com/827570/156627940-c726b6e0-b74a-4b46-ae7b-b0727cdff05b.jpeg)
![5-Heads-USB-Boot-ISO-verification-Selection-of-ISO-boot-option](https://user-images.githubusercontent.com/827570/156627944-1cc0ad56-d0b2-4552-8ee7-a159871038f7.jpeg)
![6-Heads-USB-Boot-ISO-verification-Selection-of-ISO-boot-kexec](https://user-images.githubusercontent.com/827570/156627950-6ec7e3e9-a13e-4c2c-920c-c61fbb74af1f.jpeg)


If that completes with no errors it will launch the Xen hypervisor, kernel and initrd provided from ISO and start the Qubes installer:
![7-Q41-first-screen](https://user-images.githubusercontent.com/827570/156627951-bc29e472-db90-4a01-a870-0ab2ffa70c2c.jpeg)
![8-Q41-Select-Installation-destination](https://user-images.githubusercontent.com/827570/156627954-e5681533-80cd-41b8-9b47-4ecd2cb7d132.jpeg)

Use default QubesOS partitioning scheme for QubesOS 4.x:
![9-Q41-Destination-automatic-partitioning-with-encryption](https://user-images.githubusercontent.com/827570/156627956-1dc8e56e-5a3b-403c-bf67-5235b5150ce7.jpeg)
![10-Q41-Destination-automatic-partitioning-with-encryption-done](https://user-images.githubusercontent.com/827570/156627958-b6d9e8b9-0c7b-4469-9a3b-c512b2982d41.jpeg)


The Disk Recovery Key that you enter here will be used as the
"recovery password" later.  It should be a long value since you won't
have to enter it very often; only when upgrading the Heads firmware, 
when setting a new boot default and desiring to change TPM released disk encryption 
key (Disk Unlock Key), or if there is a need to recover the disk on an external machine.
![11-Q41-Destination-automatic-partitioning-with-encryption-disk-reovery-passphrase-prompt](https://user-images.githubusercontent.com/827570/156627961-ff29a794-459e-4470-8f51-7574736d0c65.jpeg)
![12-Q41-Destination-automatic-partitioning-with-encryption-disk-reovery-passphrase-confirmation](https://user-images.githubusercontent.com/827570/156627972-64163ad3-7093-433c-bf1d-116ccbb7a3ae.jpeg)
![13-Q41-Destination-automatic-partitioning-addtitional-step-on-existing-install_reclaim](https://user-images.githubusercontent.com/827570/156627973-0b5df668-6170-403b-baf9-149499ce51d0.jpeg)
![14-Q41-Destination-automatic-partitioning-addtitional-step-on-existing-install_reclaim_delete_all](https://user-images.githubusercontent.com/827570/156627977-20ea5e55-3ff4-4473-bfeb-b71088097274.jpeg)
![15-Q41-Destination-automatic-partitioning-addtitional-step-on-existing-install_reclaim_delete_all-reclaim](https://user-images.githubusercontent.com/827570/156627978-760371ce-d0ab-4915-9601-6c0436f9e845.jpeg)
![16-Q41-user-creation](https://user-images.githubusercontent.com/827570/156627980-32ca88c1-7ea3-43d9-b7e7-f2a569f2b202.jpeg)
![17-Q41-user-creation-passphrase](https://user-images.githubusercontent.com/827570/156627984-0a833010-f11f-4ff5-823d-6e6a4e5a36e0.jpeg)
![18-Q41-Begin-installation](https://user-images.githubusercontent.com/827570/156627987-d2da1179-bc8c-4bb0-b471-05eba03efaae.jpeg)
![19-Q41-package_installation](https://user-images.githubusercontent.com/827570/156627989-90ac14aa-0374-418c-89ac-4e21750ff659.jpeg)
![20-Q41-package_installation2](https://user-images.githubusercontent.com/827570/156627992-54cdcac2-da5f-4e34-9a89-b7fd4aa53667.jpeg)
![21-Q41_installation_complete-reboot_system](https://user-images.githubusercontent.com/827570/156627998-c698ddc6-f565-4332-b31e-d87effb25035.jpeg)
![22-Heads_Options_to_boot_options](https://user-images.githubusercontent.com/827570/156628000-b48415ce-f525-4140-94b2-63c31c044dc0.jpeg)
![23-Heads_Boot_options_to_unsafe_boot](https://user-images.githubusercontent.com/827570/156628003-6d086bd9-f96d-436d-a28d-0d71b469c75e.jpeg)
![24-Heads_unsafe_boot](https://user-images.githubusercontent.com/827570/156628007-d6b8b1e1-9305-4c54-b444-519d8a77f893.jpeg)
![25-Heads_unsafe_boot_confirmation](https://user-images.githubusercontent.com/827570/156628011-000e6ca6-b3a8-4ad5-856e-07fe257fa807.jpeg)
![26-Heads_unsafe_boot_confirmation_select_dynamic_option](https://user-images.githubusercontent.com/827570/156628014-539a5819-bb61-48ca-b1f3-375fe7de3f21.jpeg)
![27-Q41_disk_recovery_key_passphrase-prompt](https://user-images.githubusercontent.com/827570/156628016-b8840c69-cb68-4419-88cf-f9b1a8b29474.jpeg)
![28-Q41_second_stage_install_main](https://user-images.githubusercontent.com/827570/156628018-889d8b46-4303-4ff0-a4cd-a2600042af83.jpeg)
![29-Q41_options_selection_done](https://user-images.githubusercontent.com/827570/156628021-d3ec34af-e620-48b8-aa39-45d84c968769.jpeg)
![30-Q41_options_selection_done_finish](https://user-images.githubusercontent.com/827570/156628023-2c49d8ac-3394-4c90-bab8-ae02afa0bf5e.jpeg)

You should now have Qubes 4.1 installed!

![Signing Qubes binaries in /boot]({{ site.baseurl }}/images/Signing_Qubes_binaries_in__boot.jpg)

Once Qubes has finished installing, you'll need to reboot and select the 'Boot
 menu' option by hitting 'm'.

Select the first boot option:

```text
1. Qubes, with Xen hypervisor [...]
```

Then make this the default boot entry by hitting 'd'.  This will also allow you
 to seal the Disk Unlock Key in TPM, if the device supports it.

You will need to input the Disk Recovery Key here (almost for the last time),
 and this should start the final stage of the Qubes installer.  Under
 `Configure Qubes` you should select `Create USB qube holding all USB controllers`
 so that they are protected from outside devices.  This step takes a little
 while as the templates are configured...

Eventually this will be done and you can click "Finish", then Qubes will
give you a login screen with your login password.

If you choose to add the Disk Unlock Key to the TPM, you'll need to specify
 which LUKS volume.  A default Qubes install will work if you leave the
 'Encrypted LVM group?' response blank and enter `/dev/sda2` when asked about
 'Encrypted devices?'.  For more details see the TPM Disk Unlock Keys
 section below. You'll then be asked to enter the Disk Recovery Key as well as
 the new boot passphrase you'll use to unseal that key.

To start Heads now (and in the future), just hit 'y' for default boot.

This should start the final stage of the Qubes installer.  Under
'Configure Qubes' you should select `Create USB qube holding all USB controllers`
 so that they are protected from outside devices.  This step takes a little
 while as the templates are configured...

Eventually this will be done and you can click "Finish", then Qubes will
give you a login screen with your login password.

After the first reboot, the boot entry will be different post-installation, so
 after you hit 'y' to select default boot you will see a message:

```text
!!! Boot entry has changed - please set a new default
```

This will also happen on OS updates that changed the boot process (updating the
   kernel or the initramfs, etc.).  If someone has tampered with your `/boot`
   partition, this can also happen, so if you're not sure of the situation,
   don't proceed.

Choose the first option again ('1'), then make it the new default ('d'), confirm
 that you're modifying the boot partition ('y'), and that you don't need to
 reseal the disk key ('n').  You'll be asked to insert your USB Security dongle
 and enter the PIN to sign the new configs and the system will reboot and allow
 you to proceed as normal.

Installing extra software
---

```shell
sudo qubes-dom0-update
```

powertop is useful for debugging power drain issues. In dom0 run:

```shell
sudo qubes-dom0-update powertop
```

You might want to make the middle button into a scroll wheel. Add this to
 `/etc/X11/xorg.conf.d/20-thinkpad-scrollwheel.conf`

 <!-- markdownlint-disable MD013 -->

```text
Section "InputClass"
  Identifier  "Trackpoint Wheel Emulation"
  MatchProduct  "TPPS/2 IBM TrackPoint|DualPoint Stick|Synaptics Inc. Composite TouchPad / TrackPoint|ThinkPad USB Keyboard with TrackPoint|USB Trackpoint pointing device|Composite TouchPad / TrackPoint"
  MatchDevicePath  "/dev/input/event*"
  Option    "EmulateWheel"    "true"
  Option    "EmulateWheelButton"  "2"
  Option    "Emulate3Buttons"  "false"
  Option    "XAxisMapping"    "6 7"
  Option    "YAxisMapping"    "4 5"
EndSection
```

<!-- markdownlint-enable MD013 -->

You'll probably want to enable fan control, as described on [ThinkWiki](http://www.thinkwiki.org/wiki/Fan_control_scripts).

Disabling the ethernet might make sense to save power

