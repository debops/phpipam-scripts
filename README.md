## phpipam-scripts

This is a set of scripts designed to access [phpIPAM](http://phpipam.net/)
database from a DHCP/DNS server.

### phpipam-hosts

`phpipam-hosts` will let you generate customized host lists which can be included
in ISC DHCP Server or dnsmasq configuration. You can also create `/etc/hosts`
and `/etc/ethers` files.

To include a generated host list in any section of the ISC DHCP Server
configuration, you can add in the dhcpd configuration file:

    include "/path/to/host-list";

To see available configuration options, run `phpipam-hosts -h`

#### Features

- generate multiple host lists from one database and automatically restart
  DHCP server if any list changed
- filter hosts by phpIPAM sections, subnets and host state (active, reserved,
  offline, dhcp)
- check MAC address validity and automatically comment out invalid hosts in
  the finished configuration file
- optionally generate host list with only invalid hosts for easier debugging

#### Requirements

- Python
- `python-mysqldb`
- installed and configured [phpIPAM](http://phpipam.net/) (can be installed on
  a different host than `phpipam-hosts`)

#### Installation

Install `phpipam-hosts` in a directory in your `$PATH`, for example
`/usr/local/sbin`. Save provided example configuration file in
`/etc/dhcp/phpipam.conf` and modify default database configuration to
reflect your environment.

`phpipam-hosts` is designed to run periodically from `cron`, access phpIPAM
database and generate specified host lists. You can either add entries with
specified arguments in cron directly (run `phpipam-hosts --help` to see list
of available options) or write your configuration in main configuration file
(`/etc/dhcp/phpipam.conf` by default) and access it using `--group`
option.

Example `cron` entry which generates a single host list and restarts `dhcpd`
daemon if host list is changed:

    */5 * * * * root /usr/local/sbin/phpipam-hosts -o /etc/dhcp/dhcpd-hosts.conf -x

You can use an example `phpipam-hosts-wrapper` script to generate multiple host
files and restart dhcpd if any one of them changes. To do that, you should
configure different host files with the same trigger file.j

### License and copyright

License: GPLv3

Copyright (C) 2014 Maciej Delmanwski <drybjed@gmail.com>  
Homepage: [https://github.com/ginas/phpipam-scripts/](https://github.com/ginas/phpipam-scripts/)  
phpIPAM homepage: [http://phpipam.net/](http://phpipam.net/)

