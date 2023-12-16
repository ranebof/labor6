ENV["VAGRANT_EXPERIMENTAL"] = "disks"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # Add disks to system
  config.vm.disk :disk, size: "400MB", name: "disk_1"
  config.vm.disk :disk, size: "400MB", name: "disk_2"
  config.vm.disk :disk, size: "400MB", name: "disk_3"
  config.vm.disk :disk, size: "400MB", name: "disk_4"

  config.vm.provision "shell", inline: <<-SHELL
    parted /dev/sdb --script mklabel gpt mkpart primary ext4 0% 100%
    parted /dev/sdc --script mklabel gpt mkpart primary ext4 0% 100%
    parted /dev/sdd --script mklabel gpt mkpart primary ext4 0% 100%
    parted /dev/sde --script mklabel gpt mkpart primary ext4 0% 100%

    pvcreate /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
    vgcreate vg00 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

    lvcreate -L 800M vg00 -n lv1
    lvcreate -l 100%FREE vg00 -n lv2

    mkfs.ext4 /dev/vg00/lv1
    mkfs.ext4 /dev/vg00/lv2

    mkdir /mnt/vol0 /mnt/vol1
    echo '/dev/vg00/lv1 /mnt/vol0 ext4 defaults 0 0' >> /etc/fstab
    echo '/dev/vg00/lv2 /mnt/vol1 ext4 defaults 0 0' >> /etc/fstab

    mount -a
  SHELL
end