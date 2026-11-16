# Vultr Optimized Cloud Compute: Which of the Five Plan Types Fits Your Workload, What Will the Monthly Bill Actually Be, and When Is the $5 Plan Enough?

Vultr Optimized Cloud Compute is the dedicated-vCPU half of Vultr's platform: each instance gets its own cores instead of sharing them, and Vultr sells those cores in four workload-shaped families plus VX1, the price-performance category. That makes five plan types sitting under a single instance type in the deploy screen. VX1 starts at **$43.80/month**; the shared-CPU Cloud Compute plans start at **$5/month**.

Below: what separates the five categories, four billing rules that push the invoice above the sticker price, the deploy steps with the three switches people get wrong, and the cases where you should skip dedicated CPU entirely.

## The short version

> Most production web and app servers land on General Purpose or VX1. Databases and caches go to Memory Optimized. Anything that spends its day compiling, encoding or running CI jobs goes to CPU Optimized. If you're running a low-traffic site, a VPN or a test box, the $5 shared plan is the correct answer and dedicated CPU is wasted money.
>
> 👉 [Compare the Optimized Cloud Compute plan families](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J)

## All five plan types under Dedicated CPU

Vultr's console puts VX1, General Purpose, CPU Optimized, Memory Optimized and Storage Optimized in the same sidebar once you select **Dedicated CPU** as the instance type. Per-plan specs and per-plan prices vary by region and change over time, so the table gives the published entry price where one exists and points to the deploy screen for the rest. Do not treat any single number here as the price of a specific configuration.

| Plan category | What it's shaped for | Workloads Vultr lists | Published entry price | Deploy |
| --- | --- | --- | --- | --- |
| VX1 | Price-performance per vCPU, dedicated resources for cost-optimized workloads | App and web tier (web serving, enterprise apps, analytics); data tier (databases, caches) | **$43.80/mo** | [Open VX1](https://www.vultr.com/products/vx1-compute?ref=9738262-9J) |
| General Purpose | Typical balance of CPU, memory and NVMe | Web and application servers, game servers, video and audio streaming, API serving | Shown at deploy time | [Open General Purpose](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J) |
| CPU Optimized | More CPU than memory or storage | Analytics processing, HPC, CI/CD, video encoding, batch processing, ad serving | Shown at deploy time | [Open CPU Optimized](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J) |
| Memory Optimized | More memory than CPU or storage | Relational databases, in-memory caches, analytics services | Shown at deploy time | [Open Memory Optimized](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J) |
| Storage Optimized | Large NVMe SSD with typical CPU and memory ratios | Transaction processing, non-relational databases, data streaming | Shown at deploy time | [Open Storage Optimized](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J) |

For contrast, here is where the shared line sits:

| Plan | vCPU model | Published entry price | Best for |
| --- | --- | --- | --- |
| Cloud Compute | Shared AMD and Intel CPUs, optional NVMe upgrade | **$5/mo** | Low-traffic sites, VPNs, game servers, test environments |
| VX1 (Optimized) | Dedicated vCPUs | **$43.80/mo** | Production workloads that need steady per-core performance |

Every Optimized instance ships with NVMe SSD storage and supports Cloud-Init for automated first-boot configuration. Linux instances you reach over SSH or the Vultr Console; Windows instances over RDP.

One pricing note, because older articles are still circulating: Vultr's March 2022 launch announcement quoted Optimized Cloud Compute at $28/month. That figure is from the launch and the current product pages don't repeat it, so treat it as history rather than a live entry price.

## VX1 is not a separate product line

Plenty of write-ups describe VX1 as something sitting alongside Optimized Cloud Compute. It doesn't. In the provisioning documentation, VX1 appears as one of the plan categories inside Dedicated CPU, next to the four workload families. Same instance type, same deploy flow, different resource profile.

Vultr markets VX1 as "up to 33% more cost-effective per vCPU, with up to 82% greater performance per dollar compared to leading price-performance optimized hyperscaler cloud compute offerings." Those are the vendor's own numbers, measured against an unnamed set of competitor plans, and there's no independent benchmark in this article to confirm them. Take them as a positioning statement rather than a measured result. The parts you can verify are concrete enough: dedicated resources, current-generation data center CPUs, and a $43.80/month published entry price.

Where VX1 fits practically: it's the category to look at first for a normal app-plus-database deployment where you want dedicated cores but don't have an unusual CPU-to-RAM ratio. General Purpose and Memory Optimized plans are also offered with enhanced-performance options if your workload profile is more specific.

## What the bill actually looks like

This is the part that decides whether the sticker price is real. Vultr's FAQ spells out several rules that most comparisons skip.

**Billing caps at 672 hours per month, minimum one hour.** A 31-day month is 744 hours, but you're only billed for 672. In practice a full month of runtime costs the same as 28 days, and short-lived instances are rounded up to the hour.

**Stopped instances keep billing.** Vultr's own answer: an instance in a stopped state still reserves RAM, SSD storage, IP addresses and vCPU, "and therefore incur charges until the virtual machine is destroyed." Powering off from the console does not pause the meter. Only destroying the instance does. If you spin up a $43.80 VX1 box for a weekend migration test and leave it stopped for three weeks, you pay for the month.

**Automatic backups add 20% to the base fee.** A $43.80/month instance with backups enabled becomes $52.56/month before you've stored a byte anywhere else.

**Snapshots cost $0.05/GB per month**, and Vultr notes the price is subject to change. A 200 GB snapshot runs about $10/month, sitting there quietly alongside the instance it came from.

**Egress beyond the included allowance is $0.01/GB worldwide.** Every instance includes a free outbound allocation; anything past it bills at that flat rate. Push an extra terabyte out in a month and that's roughly $10.

Add those together for a realistic mid-size deployment: one VX1 instance with backups, a 200 GB snapshot and a terabyte of overage lands near **$72/month**, not $43.80. None of that is hidden, but none of it is on the pricing card either.

👉 [Start a Dedicated CPU instance and see live per-plan pricing](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J)

## Deploying one: the seven steps that matter

Vultr documents the full path through the console, API, CLI and Terraform. The console route compresses to this:

1. **Products → Compute → Deploy.**
2. **Choose Dedicated CPU as the instance type.** This is the step that decides whether you get Optimized Cloud Compute at all. Pick a shared type here and the five plan categories never appear.
3. **Pick a location.** Vultr lists 34 data center regions across six continents, so put the instance near your users or your data.
4. **Choose the plan category in the sidebar** (VX1, General Purpose, CPU Optimized, Memory Optimized or Storage Optimized), then pick the specific size by vCPU, memory, storage and bandwidth.
5. **Configure Software**: a fresh OS image, a Marketplace app with its recommended image, an ISO or iPXE image, or a restore from an existing backup or snapshot. Available images include AlmaLinux, Debian, Ubuntu, Rocky Linux, Fedora, FreeBSD, Windows and Windows Core, plus one-click stacks like Docker, GitLab, WordPress, cPanel and Minecraft.
6. **Server Settings**: SSH keys, a startup script, and a firewall group.
7. **Additional Features**, where three choices are easy to get wrong:
   - **Limited User Login** creates a non-root `linuxuser` account with sudo privileges as the default login. Useful hardening, but if your automation assumes `root`, adjust it.
   - **IPv6-only** requires selecting Public IPv6 first, then deselecting Public IPv4. You can't start from an IPv4-off state.
   - **Private Instance behind NAT Gateway** routes outbound traffic through a VPC NAT gateway instead of assigning a public IP. Pick this only if you've already got the VPC and gateway in place.
8. **Hostname and label, then Deploy.** Instances come up in seconds.

The same screen also exposes DDoS protection, VPC attachment and Cloud-Init user data.

## Who shouldn't buy Optimized Cloud Compute

Dedicated vCPUs solve one problem: consistent per-core performance when a neighbour on the same host is busy. If that problem isn't yours, you're paying roughly nine times the entry price for headroom you won't use.

Shared Cloud Compute at $5/month handles low-traffic websites, VPN endpoints, game servers, staging environments and personal projects without drama. Vultr's own launch material grouped exactly those workloads under the shared tier. A blog that gets a few thousand visits a day, a WireGuard box for a small team, a test environment you rebuild weekly: none of these will notice the difference between shared and dedicated cores.

The case for dedicated gets real when you're running production web and application servers, CI/CD pipelines, video transcoding or large databases, and when a slow core shows up directly in response times or build durations. It also matters when you're selling uptime to someone else.

On the comparison question: Vultr describes Optimized Cloud Compute as "a more affordable alternative to solutions like AWS EC2." That's the company's framing, not an audited comparison, and this article doesn't include a like-for-like price table against other providers. If you need that, build it from your own instance size and region.

👉 [See what the $5 shared Cloud Compute plans include](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J)

## FAQ

**How do I log into an Optimized Cloud Compute instance?**
Linux instances accept SSH or the Vultr Console. Windows instances use RDP.

**Can I resize later?**
Yes. Vultr states that Cloud Compute instances, including the dedicated tiers, can be scaled up or down as needs change. Resizing between plan categories is a different operation from resizing within one, so check the console options for your specific instance before committing.

**AMD or Intel?**
Vultr offers both. Its own guidance: AMD EPYC-based instances deliver higher core performance and memory bandwidth for compiling, ML inference and database work; Intel-based instances suit legacy application compatibility and mixed workloads. Vultr suggests Geekbench or sysbench if you want your own numbers rather than theirs.

**Which regions are available?**
34 data center regions across six continents, with instances placed in whichever region you select at deploy time.

**Is it compliant enough for regulated work?**
Vultr cites HIPAA, SOC 2+, ISO 27001 and additional attestations, and describes a compliance posture similar to the major cloud providers. Confirm the specific certification scope for your region and workload before you rely on it.

**Can I connect it privately to my own network or another cloud?**
Yes, through VPC networking inside Vultr, or Vultr Direct Connect, which runs in partnership with Console Connect and Megaport for private links to other clouds and on-premises data centers. You can also bring your own IP space and announce it via BGP from any Vultr location.

**What do customers say about the global footprint?**
Vultr publishes a quote from Erez Zelikovitz, Executive Vice President and Chief Product and Revenue Officer at BBT.live, saying the company's model lets them "set up our Points of Presence (PoPs) on demand and expand our business to new geographies rapidly." That's a customer statement published by Vultr, not an independent review.

## Picking one, and two things to check before you pay

Match the plan type to what the workload is waiting on:

- Waiting on CPU (builds, encoding, batch jobs, analytics) → **CPU Optimized**
- Waiting on memory (MySQL, Postgres, Redis, Memcached, real-time analytics) → **Memory Optimized**
- Waiting on disk (Cassandra, MongoDB, high-frequency transaction processing) → **Storage Optimized**
- Waiting on nothing in particular, just need a solid general server with dedicated cores → **VX1** or **General Purpose**
- Not waiting on anything because traffic is light → **shared Cloud Compute at $5/month**

Two checks before you click deploy. First, the per-plan spec sheet and the price for your chosen region only exist on the deploy screen, so confirm the exact vCPU, RAM, NVMe and bandwidth numbers there rather than trusting a table. Second, remember that stopping an instance doesn't stop the billing; destroy it when you're done, or keep paying for reserved capacity you aren't using.

Vultr also runs new-account credit promotions from time to time. Whatever appears in your billing section at signup is what applies to your account, so check it before you deploy rather than after.

👉 [Open the Optimized Cloud Compute deploy page and check current per-plan pricing](https://www.vultr.com/products/optimized-cloud-compute?ref=9738262-9J)
