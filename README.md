# freebsd-update-probe.sh

## Efficiently determine if updates are required for freebsd-update.

```
freebsd-update-probe.sh efficiency is demonstrated below, see below for
before and after.

freebsd-update-probe.sh tests for a match between the current "tag" and
the upstream "tag", this test is quick and can be used to efficiently
determine if /usr/sbin/freebsd-update should subsequently be run.

/usr/sbin/freebsd-update generates the "tag" that is stored on disk and
it is authoritive.

freebsd-update-probe.sh provides a work around for FreeBSD bug:
  https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=258863
```

# Usage
```
No arguments.  Example usage:
freebsd-update-probe.sh || freebsd-update fetch [install]
  or you could
freebsd-update-probe.sh || mail_sysadmin_to_manually_update
* When /usr/sbin/freebsd-update is run you *must* ensure it completes
  successfully (exit 0) as freebsd-update-probe.sh relies on it.
* Written/tested on FreeBSD 13.0 (12.2 reported working)
* Likely to be unsuitable for FreeBSD Jail environments
Version: 20220429 ### https://github.com/tux2bsd/freebsd-update-probe 
```

# Exit codes
```
exit 0, MATCH, no freebsd-update needed.
exit 1, CHECK, freebsd-update suggested.
```

# Deploy examples:
```
fetch https://raw.githubusercontent.com/tux2bsd/freebsd-update-probe/main/freebsd-update-probe.sh -o /usr/local/bin/freebsd-update-probe.sh
chmod 700 /usr/local/bin/freebsd-update-probe.sh
# Or
fetch https://raw.githubusercontent.com/tux2bsd/freebsd-update-probe/main/freebsd-update-probe.sh -o freebsd-update-probe.sh
scp freebsd-update-probe.sh root@server.example.com:/usr/local/bin/
ssh root@server.example.com "chmod 700 /usr/local/bin/freebsd-update-probe.sh"
```

# Before (on Raspberry Pi 3B): 1m32s

```
# date ; time freebsd-update fetch install ; date
Sat Mar 26 08:41:00 NZDT 2022
src component not installed, skipped
Looking up update.FreeBSD.org mirrors... 2 mirrors found.
Fetching metadata signature for 13.0-RELEASE from update1.freebsd.org... done.
Fetching metadata index... done.
Inspecting system... done.
Preparing to download files... done.

No updates needed to update system to 13.0-RELEASE-p10.
No updates are available to install.
76.646u 22.863s 1:32.08 108.0%  22+170k 0+0io 0pf+0w
Sat Mar 26 08:42:32 NZDT 2022
```

# After (on Raspberry Pi 3B): sub 1s

```
# date ; time /root/freebsd-update-probe.sh || freebsd-update fetch install ; date
Sat Mar 26 08:43:48 NZDT 2022
probe tag file: MATCH, no freebsd-update needed.
0.095u 0.103s 0:00.51 37.2%     96+171k 0+0io 0pf+0w
Sat Mar 26 08:43:48 NZDT 2022
(minor edit: MATCH was PASS, edited for consistent README)
```

