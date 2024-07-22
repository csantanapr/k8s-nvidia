# Setup NVIDIA Jetson

## Hardware
- Jetson NVIDIA NX16GB ([Amazon](https://a.co/d/4kJ5qq5))
- seeed studio NVIDIA Jetson Orin NX 16GB Edge AI Device - reComputer J4012 Kit, 4xUSB 3.2, M.2 Key E & Key M Slot, Aluminum case, Pre-installed Jetpack System with NVIDIA JetPack on 128GB NVMe SSD
- Doesn't come with wifi but it comes with M.2 Key E slot (1x M.2 Key E for WiFi/Bluetooth module)

## Upgrades
- Hard Drive
  - AXE MEMORY 256GB NVMe M.2 2280 PCIe Gen 3x4 Internal SSD Solid State Drive (SSD) ([Amazon](https://a.co/d/5Aj0Mgk))
- Operating System (JetPack)

### Flash JetPack OS to J401 Carrier Board
- The computer came with JetPack 5, and I upgraded to JetPack 6
- Follow instructions [Flash JetPack](https://wiki.seeedstudio.com/reComputer_J4012_Flash_Jetpack/#flash-jetpack)
  - Use a jumper wire to connect the FC REC pin and the GND pin.
  - Power up the reComputer by connecting the included cable from the power adapter and connect the board with the Ubuntu host PC with a USB Type-C data transmission cable
  - On the Linux host PC, open a Terminal window and enter the command lsusb. If the returned content has one of the following outputs according to the Jetson SoM you use, then the board is in force recovery mode. Use `lsusb`
  - Download JetPack for example [JetPack 6.0 (LT4 36.3)  Jetson Orin NX 16GB](https://nv-jetson-images.oss-us-west-1.aliyuncs.com/J401/mfi_recomputer-orin-nx-16g-j401-6.0-36.3.0-2024-07-04.tar.gz?OSSAccessKeyId=LTAI5tKm7UD2hmuFW85cz42T&Expires=4874001146&Signature=f1AjN08eNjFFXI7cBpAsMvyJJPc%3D)
  - Extract the downloaded image file:
    ```
    sudo tar xpf mfi_xxxx.tar.gz
    # For example: sudo tar xpf mfi_recomputer-orin-nano-8g-j401-6.0-36.3.0-2024-06-07.tar.gz
    ```
  - Navigate to the unzipped directory and execute the following command to flash jetpack system to the NVMe SSD:
    ```
    cd mfi_xxxx
    # For example: cd mfi_recomputer-orin-j401
    sudo ./tools/kernel_flash/l4t_initrd_flash.sh --flash-only --massflash 1 --network usb0  --showlogs
    ```
    It takes a few minutes to complete (10 minutes)
  - Power off and remove jumper wire
  - Power On and follow the instructions to set username and password in Ubuntu


## IP Reservation
Get the IP Address from welcome message or running `ip a` it will be something like `192.168.x.y`

Configure your router to always give the same IP address to the device by creating a reservation

## SSH setup

Copy your public key to the jetson server
```
ssh-copy-id $username@$ip
```

As convienience setup your local ssh config by adding a new entry in `$HOME/.ssh/config` for example
```
Host jetson
    HostName 192.168.1.114
    User $username
```

Now test the ssh connection
```bash
ssh jetson
```
To setup ssh client create a key pair in your raspberry pi OS
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Copy the public key content and add the ssh key
```bash
cat ~/.ssh/id_rsa.pub
```
Add entry in Github to be able to git clone and push https://github.com/settings/ssh/new
More info in https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent


### Review kernel
Check uname
```bash
uname -a
```
expected output:
```
Linux jetson 5.15.136-tegra #1 SMP PREEMPT Wed Jul 3 22:31:57 UTC 2024 aarch64 aarch64 aarch64 GNU/Linux
```

Check the Kernel cmdline
```bash
cat /proc/cmdline
```
expected output:
```
root=PARTUUID=4b8a73c4-da72-45a0-bf8e-233d1240f538 rw rootwait rootfstype=ext4 mminit_loglevel=4 console=ttyTCU0,115200 firmware_class.path=/etc/firmware fbcon=map:0 net.ifnames=0 nospectre_bhb video=efifb:off console=tty0 bl_prof_dataptr=2031616@0x471E10000 bl_prof_ro_ptr=65536@0x471E00000
```

TBD: If cgroup needs to be added to kernel cmdline `/boot/extlinux/extlinux.conf`


## Install GH CLI
Follow the instructions here https://github.com/cli/cli/blob/trunk/docs/install_linux.md

Clone this repository
```bash
gh repo clone csantanapr/k8s-nvidia
```
Setup your name and email for git
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

## VSCode
You can use VSCode and connect to your device. Then you can install extensions.

Select "Remote Explorer" from the left side menu tasks, then select your device

Open the directory `~/` as workspace, open a new terminal, this will be a prompt in your device

You can install extentions like "Amazon Q" and use the Free tier, This give you an AI assitance in VSCode
