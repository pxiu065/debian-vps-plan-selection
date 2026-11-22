# Debian VPS guide: pick Debian 13 or 12, size the box for your workload, and see what €2–€40/month buys at Ava Hosting (full plan table + pre-order checks)

Debian 13 (Trixie) is the release to install for anything new. It has been the current stable branch since August 2025 on kernel 6.12. Debian 12 (Bookworm) has moved into LTS, with security fixes running to June 2028, which makes it the right base only if you are carrying an older stack that you cannot move yet.

At Ava Hosting, Debian VPS plans are priced in EUR and billed monthly, from **€2.00/month** for 1 vCPU and 1 GB RAM up to **€40.00/month** for 8 vCPU and 16 GB RAM. The **€10 Basic plan (2 vCPU, 4 GB RAM, 50 GB NVMe)** is the tier most small production workloads fit into without overpaying. Below: the full plan table, the version timeline with real end dates, sizing rules by workload, how these prices sit against four other providers, and the two things worth confirming before you pay.

> **Quick pick:** one site, one API, or a small Docker host → Basic at €10/month. A database with a growing working set, or a k3s cluster → Pro at €20/month. Anything smaller than that is fine for a static site, a tunnel, or a jump host, and nothing above Pro makes sense until you are running multiple tenants.
> 👉 [See the current Ava Hosting Debian VPS plan list and prices](https://my.ava.hosting/aff.php?aff=15562)

## Ava Hosting Debian VPS plans and prices

All six tiers run on KVM with dedicated vCPU allocation, dedicated RAM and NVMe storage, and all of them list unlimited transfer on a 1 Gbps port. Prices are shown as "starting from" on monthly billing, in euros.

| Plan | vCPU / RAM / NVMe | Transfer | Price (EUR, monthly) | Order |
| --- | --- | --- | --- | --- |
| Self-hosted Start | 1 vCPU / 1 GB / 15 GB | Unlimited | €2.00 | [Order Start](https://my.ava.hosting/aff.php?aff=15562) |
| Mini | 1 vCPU / 2 GB / 25 GB | Unlimited | €5.00 | [Order Mini](https://my.ava.hosting/aff.php?aff=15562) |
| Basic | 2 vCPU / 4 GB / 50 GB | Unlimited | €10.00 | [Order Basic](https://my.ava.hosting/aff.php?aff=15562) |
| Pro | 4 vCPU / 8 GB / 100 GB | Unlimited | €20.00 | [Order Pro](https://my.ava.hosting/aff.php?aff=15562) |
| Business | 6 vCPU / 12 GB / 150 GB | Unlimited | €30.00 | [Order Business](https://my.ava.hosting/aff.php?aff=15562) |
| Power | 8 vCPU / 16 GB / 200 GB | Unlimited | €40.00 | [Order Power](https://my.ava.hosting/aff.php?aff=15562) |

Three things about that table that matter more than the numbers.

The Start plan's cart page carries a free-trial marker. The length of the trial and what happens when it ends are not spelled out there, so read the terms on the order page rather than assuming.

Ava's older Debian pages still use plan names like VPS One and VPS Ultra, and quote 160 GB for the top tier where the cart says 200 GB. The cart is the current source; treat the older pages as background.

Renewal pricing is not published separately from the "starting from" figure. If you plan to pay annually, check the total at checkout rather than multiplying the monthly rate by twelve.

## Debian 13 or Debian 12? The support dates that decide it

Debian 13 is where new deployments should start. Debian 12 stays viable for another year and a half because Debian's support model is three years of full security support followed by two years of LTS, five years in total per stable release.

| Release | Kernel | Status | Security support |
| --- | --- | --- | --- |
| Debian 13 (Trixie) | 6.12 | Current stable since August 2025 | ~3 years full support, then LTS |
| Debian 12 (Bookworm) | 6.1 | Full support ended June 2026 | LTS until June 2028 |
| Debian 11 (Bullseye) | 5.10 | Released August 2021 | Five-year window closed August 2026 |
| Debian 10 (Buster) | 4.19 | Past end of life | — |

Ava Hosting publishes Debian 10, 11 and 12 template pages. Those pages still describe Bookworm as the current stable release, which was accurate when they were written and is not accurate now. Debian 13 availability is not stated on them, so confirm the template list on the order page. If Trixie is not offered yet, Bookworm is still a defensible base through June 2028, and Bullseye or Buster images should be avoided for anything new since their security windows have closed.

The stability that makes Debian a server default is structural. Package versions are fixed during a freeze before each release and then held for the life of that release, which is why a stack you deploy today behaves the same in eighteen months. The price of that predictability is old software: Bookworm ships PostgreSQL 15, Python 3.11 and OpenSSL 3.x, and it will not move to newer majors on its own. There are two escape hatches. Enable `bookworm-backports` to pull a newer kernel, container tooling or Mesa without leaving stable, and use the official release notes to upgrade in place with `apt update`, `apt full-upgrade` and a reboot when you move from one release to the next. Neither requires a reinstall.

The repository behind APT holds more than 51,000 packages, so most production stacks install from the default sources without third-party repos.

## How much vCPU and RAM each workload actually needs

**Static site, WireGuard endpoint, SSH jump host.** Start at €2.00 is enough CPU and RAM for nginx serving flat files or a single-purpose tunnel. The constraint is the 15 GB of NVMe, which Debian plus logs plus a couple of container images will start to crowd. Mini at €5.00 doubles RAM and gives you 25 GB for three euros more.

**One WordPress, Laravel or Rails site with the database on the same box.** Basic at €10.00 is the first tier with room for PHP-FPM workers alongside MariaDB or PostgreSQL. Mini will run it while traffic is low and gets uncomfortable the moment concurrent requests arrive.

**Nginx + PHP-FPM + PostgreSQL as a long-running production stack.** Basic is the floor. Move to Pro at €20.00 when the database working set outgrows what 4 GB of RAM can cache, since swap on NVMe is slower than RAM and shows up directly as query latency.

**Docker, k3s, Podman.** 4 GB is the practical floor for k3s running a real workload, and 8 GB is the comfortable size. That maps to Basic as a minimum and Pro as the tier you actually want if you are scheduling more than a couple of pods.

**Mail server.** Postfix, Dovecot, Rspamd and OpenDKIM are four persistent daemons plus spam filtering, so start at Basic and confirm outbound SMTP is permitted before you buy. A provider that blocks port 25 will make the plan useless for mail regardless of its specs.

**Multi-site hosting with a control panel.** Plesk, cPanel, ispmanager and DirectAdmin are available from Ava as paid add-ons. Panel overhead plus a dozen subscriptions is what Business at €30.00 is for, and Power's 16 GB only pays off when you are running several heavy sites in parallel.

**Build farm, APT mirror, reprepro/aptly repository.** This is disk-bound rather than CPU-bound. Power's 200 GB is the only tier where a mirror fits with room to spare.

## What €2 to €40 a month looks like next to other providers

Ava's ladder is priced in euros, so treat cross-provider comparisons as approximate. No currency conversion is applied here.

InterServer bills per slice at $3/month, with 4 slices (2 cores, 8 GB, 160 GB SSD, 8 TB transfer) at $12 and 8 slices (4 cores, 16 GB, 320 GB, 16 TB) at $24. Cloudzy advertises from $2.48/month at 50% off, and lists a 4 GB plan at $14.48 against a $28.95 list price, a 14-day money-back window and a 99.95% SLA. BuyVPS lists 1 vCPU / 2 GB / 30 GB at $7.99/month with a first-month $1 promotion that applies to its smaller tiers only. OVHcloud sells six monthly VPS tiers, VPS 1 through VPS 6, with NVMe storage and a 99.9% SLA, though prices were not published on the page reviewed.

Two pricing traps show up across all of them. Promotional rates and list prices diverge sharply: Cloudzy's 50% figure is a promotion, and the list price is the number that matters at renewal. First-month discounts are one-off: BuyVPS's $1 first month reverts to $7.99 from month two. Ava's own figures are "starting from" rates on monthly billing with no published renewal table, so the checkout total is the only number that counts.

👉 [Check the current price on Ava Hosting's Basic plan before you commit](https://my.ava.hosting/aff.php?aff=15562)

## Root access means the updates, backups and 3 a.m. pages are yours

Every Ava plan ships with full root access, and the 99.9% uptime SLA covers power and network infrastructure only. Application-layer availability stays with you. That split is standard for the price bracket, and it is worth understanding before you buy rather than after.

| Task | Self-managed VPS | Managed VPS |
| --- | --- | --- |
| OS updates | You | Provider may assist within scope |
| Security hardening | You | Provider may help with supported controls |
| Monitoring | You configure | Provider may include monitoring options |
| Backups | You configure | Provider may offer backup options |
| Troubleshooting | You own most server issues | Provider helps with server-level issues |
| Application management | You | Usually still you |

For reference, InterServer includes managed support from 8 slices upward, and OVHcloud sells higher support tiers separately. If you want someone else to hold the pager, those are the shapes to look for. Ava's published material describes self-managed infrastructure, so plan on owning the operating system.

👉 [Start with the €2.00 Self-hosted Start plan and confirm the managed scope at checkout](https://my.ava.hosting/aff.php?aff=15562)

## Setup checklist for a fresh Debian VPS

The stock Debian install is minimal, which keeps the attack surface small and means almost everything you need is a deliberate addition.

- Replace password logins with SSH keys on first boot.
- Configure nftables, the default firewall framework on recent Debian releases, before exposing anything.
- Disable services you are not using, and limit direct root login in favour of sudo.
- Set up backups, and test a restore rather than trusting the schedule.
- Monitor CPU, memory, disk, logs and uptime, even if it is a single cron job mailing you.
- Keep packages current, and review application and database security settings rather than only the OS.

The item people assume and shouldn't is automatic security updates. Whether `unattended-upgrades` is preconfigured varies by provider: one host explicitly ships a minimal Debian install without it configured, another ships it enabled by default. Nothing in Ava's published material states either way, so ask, and install and configure it yourself if the answer is no.

If you need a control panel, cPanel's official support matrix does not cover Debian. Plesk, ISPConfig, Webmin/Virtualmin and DirectAdmin all run on Debian 11 and 12, and CloudPanel and CyberPanel work on recent releases. For cPanel specifically, confirm the template and licence combination on the order page rather than assuming the add-on covers it.

## Who should not buy a Debian VPS

Skip Debian if you want the easiest first Linux experience: Ubuntu is friendlier and has more beginner-oriented tutorials. Skip it if your workload needs the newest release of a language runtime or database the week it ships, because stable-branch packages are held for years and backports only cover a subset. Skip it, and look at a managed plan instead, if you do not want to own updates, hardening, backups and monitoring.

Also skip the bigger tiers. Pro, Business and Power are not upgrades you grow into for prestige; if your monitoring never shows sustained CPU above 50% or memory pressure, you paid €10 to €30 a month for headroom you never used.

## The decision rule, and two things to confirm before paying

Match the tier to the workload, not to the price you would prefer to pay:

- Static site, tunnel or jump host → **Start, €2.00**
- One site with a database, no container orchestration → **Basic, €10.00**
- Docker or k3s with real services, or a database that needs RAM → **Pro, €20.00**
- Panel hosting across many subscriptions → **Business, €30.00**
- APT mirror, build farm, or a stack limited by disk → **Power, €40.00**

Two checks before you pay. First, which Debian template you are actually getting: 13 if it is offered, 12 if not, and never 10 or 11 for new work. Second, whether the image ships with automatic security updates configured and whether backups are included or an extra line item. Both change what your first week of ownership looks like.

👉 [Open Ava Hosting's order page and confirm the Debian template before checkout](https://my.ava.hosting/aff.php?aff=15562)
