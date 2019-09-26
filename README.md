# apt-snapshot

apt-snapshot manages constantly rolling snapshots of root filesystem allowing
rollback to a previous state.

## Installation

- Copy `apt-snapshot` to `/usr/local/sbin` :
`cp apt-snapshot /usr/local/sbin`
- Make the script executable :
`chmod +x /usr/local/sbin/apt-snapshot`
- Copy `apt-snapshot.default` to `/etc/default/apt-snapshot`
`cp apt-snapshot.default /etc/default/apt-snapshot`
- Customize `/etc/default/apt-snapshot` (see examples below).
- Select the right method to create periodic snapshots with apt-snapshot :


0. At each `apt update` invokation, interactive or batch (via
unattended-upgrades) :
Copy the provided `apt-snapshot.conf` to `/etc/apt/apt.conf.d/`
`cp apt-snapshot.conf /etc/apt/apt.conf.d/90apt-snapshot`

0. Every hour / day / week / month / year (hourly and yearly should be avoided
you're warned) :
Link `/usr/local/sbin/apt-snapshot` to
`/etc/cron.[daily|weekly|monthly]/apt-snapshot`.
Example for daily snapshot :
`ln -s /usr/local/sbin/apt-snapshot /etc/cron.daily/apt-snapshot`

If you want the cronjob to report only errors, create a line in a standard
`cron.d/apt-snapshot` file containing for example weekly runs :

    @weekly root apt-snapshot >/dev/null

## Usage

apt-snapshot keeps N rolling snapshots configured in
`/etc/default/apt-snapshot` file. In case something went wrong during an
update, rollback to a previous snapshot with :

`lvconvert --merge /dev/<VG>/<LV>_snapNNNN`
and reboot.

## Default file variables examples

### ORIGIN_VG

Volume Group name : the VG containing the Logical Volume we want to snapshot
before upgrades. On many default system installations, this is the host name or
something similar. It can be found via the following commands : `vgscan` or
`vgs`. If there's only one VG then you're done, otherwise, you should know what
you did !

### ORIGIN_LV

Logical Volume we want to snapshot before upgrades. It can be found via the
following commands : `lvs` or `lvscan`. If there's only one LV then you're done
again, otherwise, you should know what you did and find the right LV name to
use.

### PERCENT_SIZE

The size of the snapshot that we will create in % of the original LV size. If
the original LV is 10GB and `PERCENT_SIZE="50%"` then your snapshot will size 5GB
and will be able to track 5GB of changes from the original LV data.

### SNAP_DEVICES_COUNT

Non mandatory : will default to 1 if commented. The number of devices the VG
spans on. For example, if the LV is in a VG that can span to 3 disks in raid0
or raid1, then it should be set to 3.

### SNAP_DEVICES

Non mandatory : will default to the only one PV (Physical Volume) the list of
PV (Physical Volumes) devices the VG spans on separated by spaces.
For example :

    SNAP_DEVICES="/dev/sda2 /dev/sdb2"

### SNAP_HISTORY_KEEP

The number of snapshots to keep. Before each new snapshot creation, the script
will ensure that the total number of snapshot will not exceed that number by
removing the older one in a roll over mode.
