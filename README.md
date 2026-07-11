# ghostbsd-wifi-diag

Read-only WiFi diagnostic and reporting utility for GhostBSD 26.x and
FreeBSD 15.x, aimed at triaging Intel iwm(4)/iwlwifi(4) problems after
upgrades. 

The tool **never modifies the running system**. Its only writes are the
report directory and archive created in the current working directory.


## Usage

    sudo ./ghostbsd-wifi-diag

Produces:

    ghostbsd-wifi-report-YYYYMMDD-HHMMSS/          # report tree
    ghostbsd-wifi-report-YYYYMMDD-HHMMSS.tar.xz    # archive for bug reports

Exit codes: 0 clean run, 1 usage error (not root, unwritable CWD),
2 diagnostics completed with at least one FAIL finding.

## Report layout

    SUMMARY.txt          counts, likely cause, confidence, recommended actions
    diag.log             mirror of the terminal output
    analysis/results.txt every check with its explanation
    system/              uname, freebsd-version -ku, kenv, bectl, ZFS boot info
    hardware/            pciconf -lv/-l, usbconfig, camcontrol, dmidecode
    network/             ifconfig -a, netstat -rn, sockstat, arp -an
    wireless/            ifconfig -m, wlandebug, wireless dmesg slice
    kernel/              kldstat, kern.module_path, full sysctl -a
    firmware/            /boot/firmware inventory: stat, sha256, file(1) type
    packages/            pkg info/check -sa/version/audit, focused pkg info -l
    config/              loader.conf(.local), rc.conf(.local), devmatch, sysctl.conf
    logs/                full dmesg plus topic-filtered slices

## Analysis engine

Checks run along the hardware → driver → firmware → interface chain:
Intel device detection (AC/AX/CNVi classification), driver attachment,
firmware package and file presence/readability/permissions, dmesg failure
patterns (could not load firmware image, attach returned, device timeout,
failed to initialize), wlan interface presence, devmatch configuration,
loader.conf/rc.conf duplicate and conflicting variables, kernel/userland
version skew, pkg integrity, and hand-installed firmware detection. The
summary heuristics combine findings into a likely-cause narrative with a
coarse confidence estimate.

Missing optional utilities (dmidecode, wlandebug, ...) are recorded in the
report and never abort the run; individual command failures are annotated
in each capture's header.

MIT licensed.
