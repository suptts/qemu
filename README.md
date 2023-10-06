# qemu
quick emulator for Mac Intel Based

# Step#1 Install required software
brew install qemu

qemu-system-x86_64 --version

# Step#2 Create a virtual disk for a VM
mkdir -p ~/vm_sparky/
qemu-img create -f qcow2 ~/vm_sparky/sparky.qcow2 40G

# Step#3 download the installer ISO file and place it to the same directory. Once ISO file is downloaded we can start VM
qemu-system-x86_64 \
  -m 4G \
  -vga virtio \
  -display default,show-cursor=on \
  -usb \
  -device usb-tablet \
  -machine type=q35,accel=hvf \
  -smp 2 \
  -cdrom sparkylinux-2020.12-x86_64-xfce.iso \
  -drive file=sparky.qcow2,if=virtio \
  -cpu Nehalem
