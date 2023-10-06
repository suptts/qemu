# qemu
quick emulator for Mac Intel Based

# Step#1 Install required software
```
brew install qemu
qemu-system-x86_64 --version
```
# for Mac M1 or M2 use 
`qemu-system-arm -version` 

# Step#2 Create a virtual disk for a VM
```
cd ~ && mkdir -p ~/qemu && cd ~/qemu
qemu-img create -f qcow2 rocky-9.qcow2 15G
```

# We need to know about the supported CPU.
```
sysctl -n machdep.cpu.brand_string
qemu-system-x86_64 -cpu help
```

# Step#3 download the installer ISO file and place it to the same directory. 
```
brew install wget
wget https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.2-x86_64-minimal.iso
```
# We could get two file in the qemu directory once the ISO downloaed and disk file created
`ls -lthr`

# Step#4 Once ISO file is downloaded we can start VM (you can create rocky9-start.sh with below content)

```
qemu-system-x86_64 \
  -m 2G \
  -vga virtio \
  -display default,show-cursor=on \
  -usb \
  -device usb-tablet \
  -machine type=q35,accel=hvf \
  -smp 2 \
  -cdrom Rocky-9.2-x86_64-minimal.iso \
  -drive file=rocky-9.qcow2,if=virtio \
  -cpu host 
```

# For Ubuntu Linux
```
qemu-system-x86_64 \
  -m 2G \
  -vga virtio \
  -display default,show-cursor=on \
  -usb \
  -device usb-tablet \
  -machine type=q35,accel=hvf \
  -smp 2 \
  -cdrom ubuntu-20.04.6-live-server-amd64.iso \
  -drive file=ubuntu20-04.qcow2,if=virtio \
  -cpu host 
```
For Mac M2
```
qemu-system-arm \
  -m 2G \
  -vga cirrus \
  -display default,show-cursor=on \
  -usb \
  -device usb-tablet \
  -machine type=virt,accel=hvf \
  -smp 2 \
  -cdrom ubuntu-20.04.6-live-server-amd64.iso \
  -drive file=ubuntu20-04.qcow2,if=virtio \
  -cpu host
```

# then make it executeable
```
chmod +x rocky9-start.sh`
./rocky9-start.sh
```
# Note: Use Command +F to use full screen


# Run Ubuntu on Mac using QEMU 8.1
## Creat Disk
```
qemu-img create -f raw  ~/qemu/ubuntu-latest.raw 40G
or
qemu-img create -f qcow2 rocky-9.qcow2 15G
```
## Install Ubuntu Server

```
qemu-system-aarch64 \
   -monitor stdio \
   -M virt,highmem=off \
   -accel hvf \
   -cpu host \
   -smp 4 \
   -m 3000 \
   -bios QEMU_EFI.fd \
   -device virtio-gpu-pci \
   -display default,show-cursor=on \
   -device qemu-xhci \
   -device usb-kbd \
   -device usb-tablet \
   -device intel-hda \
   -device hda-duplex \
   -drive file=ubuntu-latest.raw,format=raw,if=virtio,cache=writethrough \
   -cdrom ubuntu-22.04.1-live-server-arm64.iso
```

## Start Ubuntu using QEMU
```



qemu-system-aarch64 \
   -monitor stdio \
   -M virt,highmem=off \
   -accel hvf \
   -cpu host \
   -smp 4 \
   -m 3000 \
   -bios QEMU_EFI.fd \
   -device virtio-gpu-pci \
   -display default,show-cursor=on \
   -device qemu-xhci \
   -device usb-kbd \
   -device usb-tablet \
   -device intel-hda \
   -device hda-duplex \
   -drive file=ubuntu-latest.raw,format=raw,if=virtio,cache=writethrough
```

