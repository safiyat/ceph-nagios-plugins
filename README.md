# Nagios plugins for Ceph

A collection of nagios plugins to monitor a [Ceph][] cluster.

## Authentication

Ceph is normally configured to use [cephx] to authenticate its client.

To run the `check_ceph_health` or other plugins as user `nagios` you have to create a special keyring:

    root# ceph auth get-or-create client.nagios mon 'allow r' > client.nagios.keyring

And use this keyring with the plugin:

    nagios$ ./check_ceph_health --id nagios --keyring client.nagios.keyring

## check_ceph_health

The `check_ceph_health` nagios plugin monitors the ceph cluster, and report its health.

### Usage

    usage: check_ceph_health [-h] [-e EXE] [-c CONF] [-m MONADDRESS] [-n NAME] [-i ID] [-k KEYRING] [-d]

    'ceph health' nagios plugin.

    optional arguments:
      -h, --help            show this help message and exit
      -e EXE, --exe EXE     ceph executable [/usr/bin/ceph]
      -c CONF, --conf CONF  alternative ceph conf file
      -m MONADDRESS, --monaddress MONADDRESS
                            ceph monitor address[:port]
      -i ID, --id ID        ceph client id
      -n ID, --name NAME    ceph client name
      -k KEYRING, --keyring KEYRING
                            ceph client keyring file
      -d, --detail          exec 'ceph health detail'

### Example

    nagios$ ./check_ceph_health --name client.nagios --keyring client.nagios.keyring
    HEALTH WARNING: 1 pgs degraded; 1 pgs recovering; 1 pgs stuck unclean; recovery 4448/28924462 degraded (0.015%); 2/9857830 unfound (0.000%);
    nagios$ echo $?
    1
    nagios$

## check_ceph_mon

The `check_ceph_mon` nagios plugin monitors an individual mon daemon, reporting its status.

Possible result includes OK (up), WARN (missing).

### Usage

    usage: check_ceph_mon [-h] [-e EXE] [-c CONF] [-m MONADDRESS] [-i ID]
                          [-k KEYRING] [-V] [-I MONID]

    'ceph quorum_status' nagios plugin.

    optional arguments:
      -h, --help            show this help message and exit
      -e EXE, --exe EXE     ceph executable [/usr/bin/ceph]
      -c CONF, --conf CONF  alternative ceph conf file
      -m MONADDRESS, --monaddress MONADDRESS
                            ceph monitor to use for queries (address[:port])
      -i ID, --id ID        ceph client id
      -k KEYRING, --keyring KEYRING
                            ceph client keyring file
      -V, --version         show version and exit
      -I MONID, --monid MONID
                            mon ID to be checked for availability

### Example

    nagios$ ./check_ceph_mon -I node1
    MON OK

    nagios$ ./check_ceph_mon --monid node2
    MON WARN: no mon 'node2' found in quorum

## check_ceph_osd

The `check_ceph_osd` nagios plugin monitors an individual osd daemon or host, reporting its status.

Possible result includes OK (up), WARN (down or missing).

### Usage

    usage: check_ceph_osd [-h] [-e EXE] [-c CONF] [-m MONADDRESS] [-i ID]
                         [-k KEYRING] [-V] -H HOST [-I OSDID] [-o]

    'ceph osd' nagios plugin.

    optional arguments:
      -h, --help            show this help message and exit
      -e EXE, --exe EXE     ceph executable [/usr/bin/ceph]
      -c CONF, --conf CONF  alternative ceph conf file
      -m MONADDRESS, --monaddress MONADDRESS
                            ceph monitor address[:port]
      -i ID, --id ID        ceph client id
      -k KEYRING, --keyring KEYRING
                            ceph client keyring file
      -V, --version         show version and exit
      -H HOST, --host HOST  osd host
      -I OSDID, --osdid OSDID
                            osd id
      -o, --out             check osds that are set OUT

### Example

    nagios$ ./check_ceph_osd -H 172.17.0.2 -I 0
    OSD OK

    nagios$ ./check_ceph_osd -H 172.17.0.2 -I 0
    OSD WARN: OSD.0 is down at 172.17.0.2

    nagios$ ./check_ceph_osd -H 172.17.0.2 -I 100
    OSD WARN: no OSD.100 found at host 172.17.0.2

    nagios$ ./check_ceph_osd -H 172.17.0.2
    OSD WARN: Down OSD on 172.17.0.2: osd.0

## check_ceph_rgw

The `check_ceph_rgw` nagios plugin monitors a ceph rados gateway, reporting its status and buckets usage.

Possible result includes OK (up), WARN (down or missing).

### Usage
    usage: check_ceph_rgw [-h] [-d] [-B] [-e EXE] [-c CONF] [-i ID] [-V]

    'radosgw-admin bucket stats' nagios plugin.

    optional arguments:
      -h, --help            show this help message and exit
      -d, --detail          output perf data for all buckets
      -B, --byte            output perf data in Byte instead of KB
      -e EXE, --exe EXE     radosgw-admin executable [/usr/bin/radosgw-admin]
      -c CONF, --conf CONF  alternative ceph conf file
      -i ID, --id ID        ceph client id
      -V, --version         show version and exit

### Example

    nagios$ ./check_ceph_rgw
    RGW OK: 4 buckets, 102276 KB total | /=102276KB

    nagios$ ./check_ceph_rgw --detail --byte
    RGW OK: 4 buckets, 102276 KB total | /=104730624B bucket-test1=151552B bucket-test0=12288B bucket-test2=104566784B bucket-test=0B

[ceph]: http://www.ceph.com
[cephx]: http://ceph.com/docs/master/rados/operations/authentication/

---
# Modifications



Commands for my use case:
- `ceph -s -f json-pretty` - Gives info for the **Ceph Cluster Overall Status** section.
- `ceph quorum_status -f json-pretty` - Gives info for the **Ceph Cluster Monitor Status** section.
- `ceph osd stat -f json-pretty` - Gives info for the **Ceph Cluster OSD Status** section.
- *Historical Data* - Gives info for the **Ceph Cluster Performace Metrics** section. (Refer [ceph-dash/app/templates/status.html:104](https://github.com/Crapworks/ceph-dash/blob/master/app/templates/status.html#L104) )
- `ceph pg stat -f json-pretty` - Gives info for the **Ceph Cluster Placement Group Status** section.