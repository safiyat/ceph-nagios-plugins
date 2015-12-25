# List of commands to be (or already) implemented

`ceph health` - Gives a sinle line overall health status of the ceph cluster.
> HEALTH_WARN 667 pgs degraded; 667 pgs stuck degraded; 960 pgs stuck unclean; 667 pgs stuck undersized; 667 pgs undersized; recovery 43/86 objects degraded (50.000%)

`ceph df` - Gives a 'df' of the ceph cluster.
>GLOBAL:
>    SIZE       AVAIL      RAW USED     %RAW USED 
>    14992G     14992G         564M             0 
>POOLS:
>    NAME             ID     USED     %USED     MAX AVAIL     OBJECTS 
>    rbd              0         0         0         7496G           0 
>    images           1         0         0         7496G           0 
>    volumes          2         0         0         7496G           0 
>    .rgw.root        3       848         0         7496G           3 
>    .rgw.control     4         0         0         7496G           8 
>    .rgw             5         0         0         7496G           0 
>    .rgw.gc          6         0         0         7496G          32 
>    .users.uid       7         0         0         7496G           0 

`ceph mds stat` - Gives the status of the MDS servers.
>{
>    "mdsmap": {
>        "epoch": 1,
>        "flags": 0,
>        "created": "0.000000",
>        "modified": "2015-12-21 13:24:49.728388",
>        "tableserver": 0,
>        "root": 0,
>        "session_timeout": 0,
>        "session_autoclose": 0,
>        "max_file_size": 0,
>        "last_failure": 0,
>        "last_failure_osd_epoch": 0,
>        "compat": {
>            "compat": {},
>            "ro_compat": {},
>            "incompat": {}
>        },
>        "max_mds": 0,
>        "in": [],
>        "up": {},
>        "failed": [],
>        "stopped": [],
>        "info": {},
>        "data_pools": [],
>        "metadata_pool": 0,
>        "enabled": false,
>        "fs_name": "cephfs"
>    },
>    "mdsmap_first_committed": 1,
>    "mdsmap_last_committed": 1
>}

`ceph quorum_status` - Gives the information about the monitors in the cluster.
>{
>    "election_epoch": 2,
>    "quorum": [
>        0
>    ],
>    "quorum_names": [
>        "dev-ceph-node14"
>    ],
>    "quorum_leader_name": "dev-ceph-node14",
>    "monmap": {
>        "epoch": 1,
>        "fsid": "744b9968-7b10-4251-877f-fb7e5d3f9b0a",
>        "modified": "0.000000",
>        "created": "0.000000",
>        "mons": [
>            {
>                "rank": 0,
>                "name": "dev-ceph-node14",
>                "addr": "10.41.0.94:6789\/0"
>            }
>        ]
>    }
>}

`ceph mon stat` - Gives infomation about the monitors in the cluster.
>e1: 1 mons at {dev-ceph-node14=10.41.0.94:6789/0}, election epoch 2, quorum 0 dev-ceph-node14

`ceph osd stat` - Gives the information pertaining to the OSDs.
>osdmap e87: 15 osds: 15 up, 15 in; 293 remapped pgs

`ceph pg stat` - Gives information about the PGs in the cluster.
>v9475: 960 pgs: 667 active+undersized+degraded, 215 active, 78 active+remapped; 848 bytes data, 567 MB used, 14992 GB / 14992 GB avail; 43/86 objects degraded (50.000%)


## Output formats
One can format the output as per one's preferences, using the `-f` switch.
Paramaters:
- `json` - One line JSON.
- `json-pretty` - Formatted JSON
- `xml` - One line XML
- `xml-pretty`- Formatted XML
- `plain` - Human readable
