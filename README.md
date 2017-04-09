# apt-snapshot

Manage snapshots of root filesystem before APT operations.

## Installation

- Copy `apt-snapshot` to `/usr/local/sbin`.
  `cp apt-snapshot /usr/local/sbin`
- Make the script executable
  `chmod +x /usr/local/sbin/apt-snapshot`
- Copy `apt-snapshot.default` to `/etc/default/apt-snapshot`
  `cp apt-snapshot.default /etc/default/apt-snapshot`
- Copy APT conf file to APT conf.d folder /etc/apt/apt.conf.d/
  `cp apt-snapshot.conf /etc/apt/apt.conf.d/90apt-snapshot`
- Customize `/etc/default/apt-snapshot`

## Usage

apt-snapshot is launched each `apt update` invokation ; Interactive or batch.  
apt-snapshot keeps N rolling snapshots configured in /etc/default/apt-snapshot file

## Default file variables examples

- ORIGIN_VG
  Volume Group name : the VG containing the Logical Volume we want to snapshot before upgrades.
  On many default system installations, this is the host name or something similar. It can be found via the following commands : `vgscan` or `vgs`. If there's only one VG then you're done, otherwise, you should know what you did and how you named all those VGs !
- ORIGIN_LV
  Logical Volume we want to snapshot before upgrades. It can be found via the following commands : `lvs` or `lvscan`. If there's only one LV then you're done again, otherwise, you should know what you did and find the right LV name to use.
- PERCENT_SIZE
  The size of the snapshot that we will create in % of the original LV size.
  If the original LV is 10GB and PERCENT_SIZE="50%" then your snapshot will size 5GB and will be able to track 5GB of changes from the original LV data.
- SNAP_DEVICES_COUNT
  Non mandatory : will default to 1 if commented.
  The number of devices the VG spans on. For example, if the LV is in a VG that can spdread to 3 disks in raid0 or raid1, then it should be set to 3.
- SNAP_DEVICES
  Non mandatory : will default to the only one PV (Physical Volume)
  the list of PV (Physical Volumes) devices the VG spans on separated by space.
  ex : SNAP_DEVICES="/dev/sda2 /dev/sdb2"
- SNAP_HISTORY_KEEP
  The number of snapshots to keep. Before each new snapshot creation, the script will ensure that the total number of snapshot will not exceed that number by removing the older one in a roll over mode.
