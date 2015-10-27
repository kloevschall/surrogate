### Notes regarding this fork (kloevschall/surrogate)

- Has been tested on CentOS 7.1
- MariaDB 10.1 from the official MariaDB YUM repository
- innobackupex and xtrabackup version 2.3.2 from the official Percona-Release YUM repository
  - `exclude = Percona-Server*` was added to the percona-release.repo so that the system provided mysql libs was not replaced by Percona mysql libs (creating havoc)
- qpress version 1.1 from the Percona-Release YUM repository

qpress needs to be installed via yum and not via the installer script.

I do not store mysql database and backup within the same directory! So to the prompt for "Directory to store data" I point it to the location of my backup directory. Then set `mysql_data_path` and `mysql_log_path` from within `surrogate.conf`. The location of `mysql_socket` does not match CentOS so it needs to be edited. Keep the default installation paths for config, libs, and surrogate logs to be on the safe side.

In this version `mysql_data_path` and `mysql_log_path` can be the same directory.

Creation of the crontab file is not working... Add it yourself with e.g. `crontab -e`:

 `0 8 * * * /usr/local/bin/surrogate -b full`

In the above environment full and incremental backup is working. Restoring a complete backup is working also.

But, test it yourself. Especially also the restoring process to be on the safe side.

### Warning

This project is no longer maintained actively. Use at your own risk. 

### Surrogate

A simple bash wrapper for Percona's Xtrabackup utility.

_Bring back life form. Priority One. All other priorities rescinded._

----

### Prerequisites

- Qpress, included in the installer, otherwise you can get a copy [here.](http://www.quicklz.com/qpress-11-linux-x64.tar)

- Percona 5.5+
- Percona Xtrabackup 2.0.1 or later
- Ample disk space (even with compression backups are only 2:1 ratio)

#### Usage

`sh surrogate -<flag> <argument>`

- -h	Usage
- -b	Performs a backup, either incremental or full depending on the argument you supply, for example: "surrogate -b full"
--	Accepts either "full" or "inc" as an argument
- -r  Restore using default digest location
- -c  Restore, accepts a file containing a list of directories to restore.


#### Configuration

Main configuration file
- /etc/surrogate/surrogate.conf

Xtrabackup tuning configuration (for future versions, not currently used)
- /etc/surrogate/xtrabackup.conf

#### Retention directory tree 

    /data (customizable data directory)
    |-- backups
    |   |-- daily
    |   |   |-- Fri
    |   |   |-- Mon
    |   |   |-- Sat
    |   |   |-- Sun
    |   |   |-- Thu
    |   |   |-- Tue
    |   |   `-- Wed
    |   |-- monthly
    |   `-- weekly
    |-- log
    |   `-- bin
    |-- mysql (or your my.cnf datadir)
    |-- tmp

#### Default rotation policy (configurable in surrogate.conf)

- 7 days
- 4 weeks
- 6 months

#### Authors

- [Loren Carvalho](https://github.com/sixninetynine)
- [Jesse R. Adams](https://github.com/jesseadams)

#### License

GPLv3
