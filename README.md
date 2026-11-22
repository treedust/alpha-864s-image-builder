# Ubuntu Server 26.04 LTS Autoinstall ISO — LattePanda Alpha 864s

- Alpha 864s **Ubuntu Server 26.04 (`amd64`)** autoinstaller from a USB drive.
- Pre-configured for SSH based development on **LattePanda Alpha 864s**.
- ISO layout is purely **GPT + UEFI + GRUB** intended for UEFI USB based installation.  
  > MBR/BIOS boot records are included in the ISO

## ISO Details
- **Boot Architecture:** GPT partition table with UEFI GRUB
    > Autoinstall `nocloud` (datasource) `user-data` (seedfrom) file will be under to `/cdrom/`  
    > see `mountpoint` at https://git.launchpad.net/ubuntu/+source/casper/tree/scripts/casper?h=applied/ubuntu/resolute#n7
    > or https://github.com/turnkeylinux/casper/blob/master/scripts/casper#L7  
    >  
    > The `vmlinuz` compressed kernel boot params need to contain `autoinstall ds=nocloud\;s=/cdrom/nocloud/` so that `cloud-init` will load the `user-data` seed file from the mounted USB path `/cdrom/nocloud/` and have `Subiquity` trigger the autoinstallation at boot.
    > * Unrecognized kernel boot parameters are ignored by the kernel and readable via `/proc/cmdline`.  
    >   This allows user-space services like `cloud-init` to read them at boot time.  
- **Targeted Block Device:** Installation automatically installs onto the LattePanda Alpha 864s 64GB eMMC storage `/dev/mmcblk1`.
- **Developer Board Access:**
  - Pre-configured local admin user (`devuser`) and hostname (`dev-board`).
  - Password found on https://canonical-subiquity.readthedocs-hosted.com/en/latest/howto/autoinstall-quickstart.html#create-user-data-and-meta-data-files
  - SSH enabled out for headless access.
  - static Ethernet networking on `enp2s0`:
    - `192.168.1.109/24` (gateway `192.168.1.1`) for standard router/LAN access.
  - Offline `apt` fallback enabled so unattended installs complete even without internet access.

## Flashing to USB (Important)
- **Use DD mode only.** Other methods can rewrite the partition table and result in `nocloud` not being found at boot.
- **Windows (Rufus):** Select the `.iso`, click **START**, and choose **"Write in DD Image mode"** when prompted.
  - Ensure `Partion Scheme` is GPT
- For UEFI mode `grub-pc-bin` is not used to boot the system. UEFI uses EFI system partition (ESP) and the `grub-efi-amd64` package  
  > sudo apt purge --auto-remove grub-pc-bin