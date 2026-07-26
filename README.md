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

# Software License

Copyright (c) 2026 Vester Imanuel Thacker

All rights reserved.

This software is licensed, not sold.

Permission is granted to download, install, and use the unmodified binary form of this software for any lawful purpose, subject to the following conditions:

1. You may not modify, reverse engineer, decompile, disassemble, or create derivative works of this software except where such restrictions are prohibited by applicable law.
2. You may not redistribute, sublicense, rent, lease, or sell this software without prior written permission from the copyright holder.
3. This license does not grant any rights to the source code. No source code is provided or implied.
4. All copyright, trademark, and other proprietary notices must remain intact.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NONINFRINGEMENT.

IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY CLAIM, DAMAGES, OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT, OR OTHERWISE, ARISING FROM, OUT OF, OR IN CONNECTION WITH THE SOFTWARE OR THE USE OF THE SOFTWARE.

Any rights not expressly granted by this license are reserved by the copyright holder.
