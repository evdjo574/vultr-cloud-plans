# Vultr Cloud: What It Really Costs, Which Plan You Need, and How to Claim the $300 Credit (Full Plan Comparison)

Vultr Cloud's cheapest shared-CPU instance is listed at **$0.005/hour** for 1 vCPU, 0.5 GB RAM and 10 GB storage, which Vultr's own datasheet rounds to **$2.50/month with bandwidth and storage included**. Most readers end up above that floor: dedicated-CPU Optimized Cloud Compute starts at **$30/month**, and the VX1 line starts at **$43.80/month**. GPU and bare metal pricing depends on configuration.

Below is what each tier is actually for, why Vultr's own pages quote three different entry prices, and how to pick a plan without paying for capacity you'll never use.

> **Quick pick:** A website, API, small database or dev environment fits on Cloud Compute at $0.005/hour. A production database or steady traffic needs dedicated CPU, which starts at $30/month. Training or serving models means going straight to Cloud GPU. New accounts can claim the **$300 credit** through the promo link below.
>
> 👉 [See current Vultr Cloud Compute pricing and the $300 credit](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J)

## Every Vultr Cloud plan family, side by side

Vultr Cloud is an umbrella brand, not a single product. Cloud Compute is the shared-CPU virtual machine line; everything else is a separate family with its own pricing logic.

| Plan family | Hardware | Starting price | Best for | Pricing |
| --- | --- | --- | --- | --- |
| Cloud Compute – Regular Performance | Shared AMD/Intel CPUs | $0.005/hr (~$2.50/mo), bandwidth and storage included | Low-traffic sites, blogs, dev/test, small CMS | [Cloud Compute pricing](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J) |
| Cloud Compute – High Performance | Shared AMD/Intel, NVMe SSD | From $2.50/mo (homepage quotes $5/mo for NVMe configurations) | Busier web apps, APIs, staging environments | [Cloud Compute pricing](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J) |
| High Frequency Compute | 3+ GHz processors on every plan | Configuration-dependent | Single-core-bound apps, latency-sensitive services | [Full price list](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Optimized Cloud Compute | Dedicated AMD EPYC, NVMe SSD, bandwidth included | $30/mo | Production databases, e-commerce, game servers, streaming, real-time analytics | [Full price list](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Vultr VX1 | Latest-generation data center CPUs, local NVMe or Block Storage boot drives | $43.80/mo | Cost-sensitive app-tier and data-tier workloads | [VX1 plan pricing](https://www.vultr.com/products/vx1-compute/?ref=9738262-9J) |
| Cloud GPU | NVIDIA A16, A40, A100, L40S, HGX B200, GH200; AMD Instinct MI300X, MI325X, MI355X | Configuration-dependent | Model training, inference, visualization, HPC | [Full price list](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Bare Metal | Single-tenant physical servers, no resource sharing | Configuration-dependent | Workloads that need the whole machine | [Full price list](https://www.vultr.com/pricing/?ref=9738262-9J) |

Two things worth noting in that table. Bandwidth and storage are folded into the quoted price on Cloud Compute and Optimized Cloud Compute, which is not how the large hyperscalers bill, so the sticker price is closer to the final invoice than you may be used to. And the gap between the cheapest shared instance and the cheapest dedicated one is roughly twelvefold, which is the single most important number when you're deciding where to start.

## What $0.005 per hour actually buys

The entry configuration is 1 vCPU, 0.5 GB RAM and 10 GB of storage. That runs a static site, a redirect, a monitoring probe, a DNS secondary, a cron box or a personal Git remote without complaint. It will not run a WordPress install with a dozen plugins, a Postgres instance you care about, or anything with a JVM in it.

The realistic upgrade path looks like this:

- **Personal blog or brochure site:** the $2.50/month tier is enough. Move up when your database starts swapping.
- **Small web app or API with real users:** High Performance Cloud Compute on NVMe, sized at 2–4 GB RAM.
- **Anything where a slow query costs money:** Optimized Cloud Compute. Dedicated cores mean no noisy neighbours stealing cycles at peak traffic.
- **Steady, predictable app-tier traffic at the lowest cost per vCPU:** VX1.

Vultr says instances can be scaled up or down without infrastructure disruption, and in practice that means resizing rather than rebuilding. Start small; the cost of being wrong for a month is a few dollars.

👉 [Compare the entry-level Cloud Compute configurations](https://www.vultr.com/pricing/?ref=9738262-9J)

## Why Vultr's own pages quote three different starting prices

You'll see $0.005/hour in the docs, $2.50/month in the product datasheet, and $5/month on the homepage. None of these is wrong. They describe different configurations: the hourly figure is the raw metered rate for the smallest instance, the monthly figure is the same class of plan with bandwidth and storage bundled, and the $5 figure refers to plans that include the NVMe SSD upgrade.

The lesson for budgeting is simple: read the price attached to the specific configuration you're ordering, not the headline on the marketing page. Hourly billing with a monthly cap means a short-lived test costs cents, but a 4 GB instance left running for thirty days bills for thirty days whether or not anyone visits the site.

## Where the vendor's performance claims come from

Vultr states that VX1 is up to 33% more cost-effective per vCPU and delivers up to 82% greater performance per dollar than "leading price-performance optimized hyperscaler cloud compute offerings." Those are vendor benchmarks against unnamed competitor configurations, so treat them as directional rather than as an independent test result. What is verifiable is the positioning: VX1 exists for app-tier and data-tier workloads that need reliable, economical throughput rather than the fastest possible single core. If your workload is a compile job or a latency-critical API, High Frequency's 3+ GHz processors are the relevant line, not VX1.

## The infrastructure details that don't make the marketing pages

Vultr currently lists **34 cloud data center regions** across six continents, with straightforward global bandwidth pricing rather than regional egress tiers. That matters if you're serving users in more than one country: you can place instances near them and keep the bill predictable.

A few capabilities that are easy to miss when comparing providers:

- **VPC-Only Instances.** You can deploy VMs with private IPs only, reachable through a Virtual Private Cloud and NAT Gateway, so a database tier never touches the public internet.
- **Reserved IPs survive instance destruction.** Tear down a server and keep the address for the replacement.
- **Bring your own IP space.** BGP announcement of your own range is available in every Vultr location.
- **Layer 3 and layer 4 DDoS mitigation** is part of the platform, with a web-based firewall you can enable per instance.
- **Snapshots and scheduled backups** are configured from the console, which is what makes the resize-and-move-on workflow practical.

## Automation, clustering and one-click deployments

Everything in the console is also available through the API and a Terraform provider, which is the part that matters if you're managing more than a handful of machines. Vultr Clusters lets you create and modify groups of compute instances on demand, with no reservations or special requests required, so capacity planning is a matter of scaling out rather than negotiating.

For getting software running, the Marketplace covers one-click images for the usual stack (WordPress, Docker, GitLab, cPanel, Drupal, Joomla, Minecraft and others), and the Container Registry can sync public images worldwide with a Docker Hub proxy cache to speed up pulls. Operating system choice runs from AlmaLinux, Rocky, Debian, Ubuntu and Fedora through FreeBSD, Windows and Windows Core, plus custom ISOs.

## If you're migrating off AWS, Azure or Google Cloud

Vultr publishes step-by-step migration guides for moving EC2 instances, Google Compute Engine instances and Azure virtual machines onto Cloud Compute, covering application data, configuration and cutover validation. That's a documented path rather than a lift-and-shift promise, and it's the thing to read before you commit a production workload, because the parts that break in a cloud migration are usually IAM, networking and DNS rather than the server itself.

## Who shouldn't move to Vultr Cloud

You get root access and a blank machine. If you want someone else to patch WordPress, manage your PHP version and handle support tickets about plugin conflicts, managed WordPress hosting is the better purchase, even at three times the price. Vultr is infrastructure: the value comes from paying for compute rather than for a control panel.

Likewise, if your team has no one comfortable with SSH, firewalls and backup schedules, the $2.50/month instance will cost you far more in time than the $30/month managed alternative. Vultr does offer managed database services, which removes one piece of that burden, but it isn't a fully managed application platform.

The other case worth naming: workloads that need a large, tightly coupled GPU cluster for weeks of training. Vultr's GPU lineup covers A100, L40S, HGX B200, GH200 and AMD's MI300X/MI325X/MI355X, and there's a GPU supercluster build in Chennai, but for a single long training run you should price the specific GPU configuration and region rather than assume the general-purpose pricing applies.

👉 [Check Vultr's GPU configurations and available regions](https://www.vultr.com/pricing/?ref=9738262-9J)

## Questions people ask before signing up

**Is Vultr Cloud the same as Vultr Cloud Compute?**
No. Vultr Cloud is the brand covering compute, GPU, bare metal, storage, networking and managed services. Cloud Compute specifically means the shared-CPU virtual machine line starting at $0.005/hour.

**Can I run Windows?**
Yes, Windows and Windows Core images are available alongside the Linux distributions and FreeBSD. Access is via RDP for Windows instances and SSH or the web console for Linux.

**What happens to my IP address if I delete a server?**
Reserved IP addresses remain in your account after the instance is destroyed, so you can attach them to a replacement.

**Do I have to reserve capacity in advance?**
No. Instances and clusters are provisioned on demand through the console or API, with no reservations or special requests.

**Does the $300 credit apply to everything?**
The promo link below carries Vultr's $300 new-account credit offer. The terms, including eligibility, expiry and which products the credit covers, are shown during signup. Read them before you build a plan around the credit.

## How to decide, in one pass

Match the workload, not the price list:

- **Static site, dev box, monitoring, DNS:** the $0.005/hour Cloud Compute instance. Nothing above it earns its price here.
- **WordPress, a SaaS app or an API with paying users:** High Performance Cloud Compute with NVMe, 2–4 GB RAM.
- **Production database, game server, streaming or analytics:** Optimized Cloud Compute at $30/month and up. The dedicated cores are the reason you're paying.
- **High-volume app tier where cost per vCPU is the metric:** VX1 at $43.80/month and up.
- **Anything involving model training or inference:** Cloud GPU, priced by configuration.

Two things to confirm before you click through. First, check the price for the exact configuration and region you intend to run, because the headline number belongs to the smallest instance. Second, decide whether you're paying monthly or hourly and how long the machine will actually stay up; a forgotten test instance is the most common line item on a surprise cloud bill.

👉 [Start with Vultr Cloud Compute and claim the $300 credit](https://bit.ly/VuLtr)
