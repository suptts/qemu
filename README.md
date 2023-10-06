# qemu
quick emulator for Mac Intel Based

# Step#1 Install required software
```
brew install qemu
qemu-system-x86_64 --version
```
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
# then make it executeable
```
chmod +x rocky9-start.sh`
./rocky9-start.sh
```
# Note: Use Command +F to use full screen
