# apt-snapshot

Creates a snapshot of root filesystem before APT operation.

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
