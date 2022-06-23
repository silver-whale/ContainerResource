VNC Reference: https://www.2cpu.co.kr/network/5407?&page=4

Basic Server Access: ssh {userName}@{serverIP} -p {PortNumber}

After VNC PortForwarding: ssh -x -L {PortNumber}:{LocalHostIP}:{PortNumber} {userName}@{ServerIP} -p {PortNumber}

// QEMU Reference: https://blackinkgj.github.io/qemu-zns/

QEMU Install:
udo qemu-system-x86_64 \
-hda ubuntu.img \
-boot d \
-cdrom ubuntu-20.04.4-live-server-amd64.iso \
-m $(expr $(grep MemTotal /proc/meminfo | awk '{print $2}') / $(expr 1024 '*' 1024))G -smp $(nproc) \
-cpu host \
--enable-kvm \
-vnc :{VNCnumber}

QEMU Server with ZNS SSD Open:
ZNS_SSD_IMG = zns.raw //zns.raw should be created before this command
SSH_PORT = {PortNumber}
sudo qemu-system-x86_64 \
-hda ubuntu.img \
-m $(expr $(grep MemTotal /proc/meminfo | awk '{print $2}') / $(expr 1024 '*' 1024))G \
-smp $(nproc) \
-cpu host \
--enable-kvm \
-device nvme,id=nvme0,serial=deadbeef,zoned.zasl=5 -drive file=${ZNS_SSD_IMG},id=nvmezns0,format=raw,if=none -device nvme-ns,drive=nvmezns0,bus=nvme0,nsid=1,logical_block_size=4096,physical_block_size=4096,zoned=true,zoned.zone_size=64M,zoned.zone_capacity=62M,zoned.max_open=16,zoned.max_active=32,uuid=5e40ec5f-eeb6-4317-bc5e-c919796a5f79 \
-net user,hostfwd=tcp::${SSH_PORT}-:22 \
-net nic \
-vnc :{VNCnumber}

QEMU Server Access: ssh {qemuName}@127.0.0.1 -p {Port}


// Reference: https://askubuntu.com/questions/178909/not-enough-space-in-var-cache-apt-archives
Pacakge install when storage(/var/cache) is full:
sudo mv -i /var/cache/apt /media/{directory}
sudo ln -s /media/{directory}/apt /var/cache/apt
DO INSTALL
sudo apt-get clean
sudo unlink /var/cache/apt
sudo mv /media/{directory}/apt /var/cache

Making DISC:
sudo mkdir /media/{directory}
sudo mount -t tmpfs tmpfs /media/{directory}
sudo ln -s /media/{directory}/apt /var/cache/apt

//Check process's pid
ps -ef | grep {ProcessName}