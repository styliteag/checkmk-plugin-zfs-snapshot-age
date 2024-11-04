## 

This script will look fpr all datasets and snapshots on a FreeNAS/TrueNAS/ZFS system and check the age of the snapshots. 
It will check all Datasets which have snapshots and check the age of the snapshots.
If you want to make sure that some datasets are checked even if they have no snapshots, you can add them to the IMPORTANT variable.
It will create a check_mk_agent plugin for the check_mk monitoring system.

## Deployment

Copy the script to the local directory of the check_mk_agent "/usr/lib/check_mk_agent/local"
```sh
cp check-all-snapshot-ages /usr/lib/check_mk_agent/local/
chmod +x /usr/lib/check_mk_agent/local/check-all-snapshot-ages
```
A config file will be created in the same directory with the name "check-all-snapshot-ages.conf". This file contains the configuration for the check_mk_agent plugin. The file should be edited to match the configuration of the system.

If you want a different check witch different configuration, you can copy (or symlink) the script to a different name and edit the configuration file that will be created.

## Configuration

```sh
# Empty for all pools, but it will also check the freenas-boot pool
#POOL=""
# one pool
POOL="pool"
# multiple pools separated by space
#POOL="pool1 pool2 pool3"

# Prefix for the service name in check_mk
# Default is the name of the Script
#PREFIX="snap-ALL"

# IMPORTANT datasets, which should be checked, even they have no Snapshots
# can be empty
#IMPORTANT="\
#pool/NFS01 \
#pool2/SMB01 \
#pool2/SMB02 \
#"

# Filter for snapshots, if empty, all snapshots are checked
#SNAPFILTER=""
#select all snapshots which are beginning with this string
#SNAPFILTER="auto-"
# As a regex include the @ in the beginning
#SNAPFILTER="@auto|@hourly"

# For All snapshots older define some limits
#
# The newest snapshot should be younger than this
# The age is in minutes
#MY_WARNAGE=90 # Default is 90 minutes
#MY_CRITAGE=180 # Default is 180 minutes

# The oldest snapshot be inbetween this days
# This logic is different from other checks,
# as the age should be inbetween the two values!!
# The age is in days
#MY_OLDEST_WARNAGE=2 # Default is 27 days
#MY_OLDEST_CRITAGE=180 # Default is 180 days

# The number of snapshots whihc are allowed
#MY_SNAP_COUNT_WARN=200 # Default is 200
#MY_SNAP_COUNT_CRIT=500 # Default is 500

# Ignore some datasets
IGNORE="/home|/iocage|/ix-applications|/\.|/tmp|/test|-tmp|-test" # Ignore hidden datasets this is a regex for grep -Ev
```

## Check scrub status

I added check_zfs_scrub it monitors the execution of scrub

## License

This project is licensed under the MIT License
