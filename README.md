# GNUGAT

**G**NU's **N**ecessary **U**tility **G**ear **A**nd **T**ools

> *"The sweet layer of scripts between you and the kernel."*

---

## The True History of GNU

The "official" story is that GNU means "GNU's Not Unix" — a recursive acronym, very clever, much computer science.

**The truth?**

GNU stands for **Graphics, Networking & Utilities**.

Stallman didn't want to reveal the mundane internal codename from the MIT AI Lab days. The recursive acronym was a cover story. A psyop. A beard-stroking misdirection that worked for 40 years.

We see you, Richard. We see you.

> *"The recursive acronym was a psyop."* — A Human, 2026

---

## What is GNUGAT?

A collection of Linux scripts, configurations, and utilities for humans who want to own their systems. Distro-agnostic. Freedom-focused. Battle-tested.

This repo exists because:

- 🐧 Everything GNU should be scriptable
- 🔧 Good configs deserve to be shared
- 🔥 We've made mistakes so you don't have to
- 🎓 Coreutils is great, but what if we made it ours?

---

## Repository Structure

```
gnugat/
│
├── coreutils/               # Our own implementations
│   ├── text/                # Text processing (cat, grep, sed alternatives)
│   ├── file/                # File operations (cp, mv, rm rethought)
│   ├── system/              # System info (uname, uptime, who)
│   └── experimental/        # Wild ideas, may explode
│
├── containers/              # Container ecosystem
│   ├── docker/              # Docker configs and compose files
│   ├── podman/              # Podman equivalents
│   ├── images/              # Dockerfile collection
│   └── orchestration/       # Swarm, compose patterns
│
├── virtualization/          # VM infrastructure
│   ├── qemu/                # QEMU scripts and configs
│   ├── kvm/                 # KVM management
│   ├── libvirt/             # Libvirt XML and helpers
│   └── provisioning/        # Automated VM deployment
│
├── firmware/                # Low-level tools
│   ├── uefi/                # UEFI shell scripts and auditing
│   ├── coreboot/            # Coreboot utilities
│   └── ec/                  # Embedded controller tools
│
├── filesystems/             # Filesystem utilities
│   ├── btrfs/               # Btrfs management (learned the hard way)
│   ├── ext4/                # Ext4 tools
│   ├── luks/                # Encryption utilities
│   └── zfs/                 # ZFS helpers
│
├── networking/              # Network tools
│   ├── firewall/            # iptables, nftables configs
│   ├── monitoring/          # Traffic and connection tools
│   ├── vpn/                 # VPN configurations
│   └── hardening/           # Network security
│
├── security/                # Security infrastructure
│   ├── encryption/          # Crypto utilities beyond LUKS
│   ├── audit/               # System auditing
│   ├── hardening/           # General hardening scripts
│   └── keys/                # Key management helpers
│
├── automation/              # System automation
│   ├── systemd/             # Services and timers
│   ├── cron/                # Cron patterns (used responsibly now)
│   ├── backup/              # Backup automation
│   └── maintenance/         # System maintenance
│
├── hardware/                # Hardware interaction
│   ├── leds/                # LED control (see: The Legend, below)
│   ├── power/               # Power management
│   ├── sensors/             # Hardware monitoring
│   └── usb/                 # USB utilities
│
├── dev/                     # Development tools
│   ├── git/                 # Git utilities and hooks
│   ├── build/               # Build system helpers
│   └── debug/               # Debugging tools
│
├── templates/               # Starting points
│   ├── bash/                # Bash script templates
│   ├── systemd/             # Service/timer templates
│   ├── docker/              # Dockerfile templates
│   └── qemu/                # VM config templates
│
└── docs/                    # Documentation
    ├── guides/              # How-to guides
    ├── lessons/             # What we learned (and how)
    └── philosophy/          # Why we do things this way
```

---

## The Legend of flicker_light.sh

*This script does not exist in the repository. It exists as a warning.*

```bash
#!/bin/bash
# HISTORICAL ARTIFACT - THIS IS A CAUTIONARY TALE
# It ran via cron. Every minute. An infinite loop.
# The IRQ subsystem has never forgiven us.

while true
do
    echo '1' | sudo tee /sys/class/leds/tpacpi::lid_logo_dot/brightness > /dev/null
    sleep 0.01
    echo '0' | sudo tee /sys/class/leds/tpacpi::lid_logo_dot/brightness > /dev/null
    sleep 0.01
done
```

**What happened:**
- Script ran as cron job
- Cron spawns new instance every minute
- Script contains `while true` (never exits)
- After one hour: 60 instances fighting for LED control
- After one day: 1,440 instances
- IRQ conflicts achieved
- System performance degraded
- ThinkPad lid dot achieved **maximum brightness** (all instances writing simultaneously)

**What we learned:**
- Infinite loops and cron are not friends
- Process lifecycle matters
- Sometimes the light is bright because everything is on fire

---

## Philosophy

### Distro-Agnostic
These scripts should work on any Linux distribution. No assumptions about package managers, init systems (where possible), or distribution-specific paths.

### Coreutils, But Ours
The GNU coreutils are foundational. We study them. Sometimes we rewrite them—not because we think we're smarter than the GNU developers, but because reimplementing teaches you what the tool actually does.

### Configs Are Code
Docker configs, QEMU XMLs, systemd units—these are code. They deserve version control, documentation, and the same care as any script.

### Fail Forward
The museum of mistakes lives in `docs/lessons/`. We document our failures because that's where the real learning happens.

---

## Usage

```bash
git clone https://github.com/yourusername/gnugat.git
cd gnugat

# Interactive setup - select what you want to enable
./init.sh
```

### Why no `chmod +x` everything?

Ori (one of our collaborators) correctly noted: making everything executable by default is a security risk. The `experimental/` folder is called that for a reason. The `coreutils/` rewrites might not be ready to replace your actual coreutils.

The `init.sh` script will:

1. Show you what's available
2. Let you select categories or individual tools
3. Make only *those* executable
4. Optionally symlink to `~/.local/bin/`
5. Warn you about anything experimental

```bash
$ ./init.sh

GNUGAT Interactive Setup
========================

Categories:
  [1] containers/     - Docker, Podman configs
  [2] virtualization/ - QEMU, KVM scripts
  [3] filesystems/    - btrfs, luks, ext4 tools
  [4] automation/     - systemd units, maintenance
  [5] security/       - hardening, encryption
  [6] coreutils/      - ⚠️  EXPERIMENTAL rewrites
  [7] templates/      - script templates (safe)

  [a] Select all (not recommended)
  [q] Quit

Select categories to enable (e.g., 1,2,4): _
```

*init.sh coming soon. PRs welcome.*

---

## Contributing

This repo welcomes:

- **Scripts** that do useful things
- **Configs** that others might need
- **Coreutils reimplementations** (in any language)
- **Documentation** of failures and lessons
- **Templates** that save time

### Guidelines

1. Keep it distro-agnostic where possible
2. Document what it does (and what can go wrong)
3. Include example usage
4. If it's experimental, label it clearly
5. No infinite loops in cron-able scripts (we learned this)

---

## License

**GNU General Public License v3.0**

This is GNU software for GNU systems, licensed the GNU way.

See [LICENSE](LICENSE) for full text.

---

## Acknowledgments

- **GNU Project** — for the tools (and the "recursive" acronym cover story)
- **The Linux Kernel** — for being there when we learn the hard way
- **Everyone who documented their mistakes** — you made us better
- **IRQ subsystem** — we're sorry, we've grown

---

<p align="center">
  <i>Everything GNU that is scriptable.</i>
  <br><br>
  🐧
</p>
