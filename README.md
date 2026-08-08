# GRE DDoS Protection Service: Built-In 100Gbps Mitigation, Remote Network Shielding With No Hardware Required

If you've ever watched your server get flattened by a 40Gbps flood at 2 a.m. on a Saturday, you already know why people go searching for a reliable GRE DDoS protection service. It's one of those topics that sounds deeply technical until you realize it's really just a question of plumbing: how do you get attack traffic cleaned *before* it hits your network, without ripping apart the infrastructure you already paid for?

That's where GRE tunneling paired with BGP comes in, and honestly, it's a more elegant trick than most people give it credit for. Let me walk through how it actually works, why it's become the go-to pattern for remote DDoS scrubbing, and where Sharktech's Remote Network DDoS Protection fits into the picture if you're shopping for a provider that's been doing this since 2003.

## What a GRE DDoS Protection Service Actually Does

Here's the short version. GRE stands for Generic Routing Encapsulation, which is a fancy way of saying "wrap one packet inside another and ship it across the internet." When you pair it with BGP for DDoS mitigation, the flow looks something like this:

1. Your router establishes a BGP session with the scrubbing provider's router and announces your IP prefixes (you need at least a `/24` block).
2. The provider announces those prefixes upstream, so inbound traffic headed for your network flows into their scrubbing centers first.
3. Their mitigation systems inspect the traffic in real time and filter out the malicious packets.
4. Clean traffic gets encapsulated into GRE and tunneled back to you over a logical link.
5. Your server responds normally on its local path, so the traffic path is asymmetric — only ingress goes through the scrubber, which roughly halves the latency impact compared to a symmetric setup.

The beauty of it is that you don't have to move your servers, buy new hardware, or re-architect anything. You just point your routes at someone who's built to absorb garbage traffic. The tunnel is transparent to your applications.

There are trade-offs, of course. GRE adds overhead, so your upstream MTU needs to be at least 1550 to accommodate the encapsulation header without fragmentation. And if the tunnel endpoints are misconfigured, you can end up with problems no scrubbing system can fix. But compared to the cost of buying your own mitigation appliances and bulking up your transit commits, BGP/GRE is usually the saner path.

## Sharktech's Take on Remote DDoS Protection

Sharktech is one of those providers that started life as a DDoS mitigation company and only later grew into a full hosting business, which is a useful origin story when you're evaluating who to trust with attack traffic. They run five data centers — Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam — connected by 1.1Tbps of total capacity, all on native 40G/100G networking.

Their Remote Network DDoS Protection is exactly the BGP + GRE + Anycast pattern described above. The mechanics, straight from their own documentation:

- A BGP session is established between your router (a soft router is fine, no hardware required) and theirs.
- You hand over your prefix list.
- A GRE tunnel is brought up between your network and theirs.
- When you announce your prefix, their network advertises it upstream and routes inbound traffic through their scrubbing systems.
- Attacks are detected and filtered at the edge; clean traffic comes back to you through the GRE tunnel.
- You can run scrubbing always-on or only when an attack is detected — your call.

According to Sharktech, they've yet to receive an attack they couldn't mitigate, thanks to a layered approach that spreads large floods across all five data centers using BGP and their total pooled bandwidth. Each facility has at least 1Tbps of connectivity, and they can adjust upstream routing dynamically to handle attack patterns that change shape mid-stream.

👉 [Check out Sharktech's Remote Network DDoS Protection setup](https://bit.ly/SharKTech)

## What It Defends Against

Sharktech publishes a fairly long list of attack types their mitigation handles, which is useful if you're trying to figure out whether your specific threat model is covered. The list includes:

- UDP Flood, ICMP Flood, ACK Flood, TCP SYN Flood, SYN-ACK-ACK
- HTTP Flood, HTTP POST Flood, Slowloris
- NTP Amplification, DNS Amplification, SSDP Reflection, MemCached Reflection, SNMP Reflection, Chargen Reflection
- UDP Reflection, Reflected ICMP & UDP, ICMP + UDP Flood
- NXDomain, Ping of Death, Smurf

In practical terms, that covers the vast majority of volumetric and protocol-layer attacks that show up in the wild. For game server operators — a segment Sharktech is openly popular with — the common 3–8Gbps floods that used to knock boxes offline get absorbed without the server skipping a beat, based on multiple customer accounts.

## Pricing: 100Gbps Protection at $39/IP, Free With Hosted Services

This is the part most people actually care about. Sharktech's pricing model for DDoS protection is unusually straightforward, and it's worth understanding the tiers because they map to very different use cases.

**DDoS protection included free with all hosted services.** If you're renting a Smart VPS or a dedicated bare-metal server from Sharktech, mitigation is baked into the base price. Standard capacity on VPS plans is 60Gbps, and dedicated hardware can go higher.

**100Gbps DDoS protection as an add-on: $39/month per IP.** After a round of router upgrades across all facilities, Sharktech dropped the 100Gbps protection price to $39 per IP, per month. It can be added to any dedicated or colocated server. The 100G IPs are assigned from Anycast-advertised prefixes, so incoming attacks get spread across all five data centers using BGP path selection — which is how a single IP can survive a flood that would overwhelm any one location.

**Remote Network DDoS Protection for external infrastructure.** If your servers live somewhere else (another provider, your own cage, an ISP you're peering with) and you just want Sharktech's scrubbing in front of it, the Remote DDoS Protection product is the one to ask about. Pricing on this is customized based on your prefix size and traffic profile — you'll want to get a quote from their sales team rather than rely on a published number.

👉 [Get a free DDoS protection consultation from Sharktech](https://bit.ly/SharKTech)

## Smart VPS Plans With Built-In DDoS Protection

If you're starting from scratch and want hosting plus mitigation in one bill, the Smart VPS line is the most common entry point. All plans run on Proxmox clusters with Xeon Gold CPUs, enterprise NVMe storage, and triple-redundant 99.999% uptime. Every tier includes 60Gbps DDoS protection at no extra cost.

The billing cycle discounts are automatic — no coupon hunting required:

| Plan | vCPU | RAM | NVMe | Bandwidth | Monthly | Annual (50% off) | Get It |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tiny | 1 core | 2 GB | 40 GB | 4 TB | $7.95/mo | [$3.98/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=smart-vps-tiny) |  |
| Small | 2 cores | 4 GB | 80 GB | 8 TB | $15.95/mo | [$7.98/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=smart-vps-small) |  |
| Medium | 2 cores | 8 GB | 160 GB | 16 TB | $31.95/mo | [$15.98/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=smart-vps-medium) |  |
| Large | 4 cores | 16 GB | 320 GB | 32 TB | $63.95/mo | [$31.98/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=smart-vps-large) |  |
| XL | 4 cores | 32 GB | 640 GB | 64 TB | $127.95/mo | [$63.98/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=smart-vps-xl) |  |

All plans include 1 IPv4 address by default (additional IPs available at order time), 1Gbps port speed, multi-region deployment across LA, Denver, Chicago, and Amsterdam, and your choice of Linux or Windows. Annual billing is the best value by a wide margin — half price, applied automatically when you select the annual option at checkout.

👉 [Browse all Smart VPS configurations](https://bit.ly/SharKTech)

## Dedicated Servers: DDoS Protection Included, 100Gbps Optional

For workloads that need bare metal — game servers handling sustained attack traffic, high-traffic applications, anything where you want direct hardware access — Sharktech's dedicated servers come with DDoS protection included by default, and you can layer the 100Gbps service on top at $39/IP.

Sample configurations and starting prices (these vary by location and current inventory):

| Configuration | RAM | Storage | Network | DDoS Protection | Starting Price | Get It |
| --- | --- | --- | --- | --- | --- | --- |
| Intel Xeon E3-1270v5 | 16 GB | 2TB HDD or 120GB SSD | 1Gbps, 30TB bandwidth | 60Gbps included | [$99/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=470) |  |
| Dual Xeon E5-2637v2 | 32 GB | 2TB HDD or 120GB SSD | 1Gbps, 30TB bandwidth | 60Gbps included | [$183.20/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=473) |  |
| Dual Xeon E5-2670 (1Gbps unmetered) | 32 GB | 2TB HDD or 120GB SSD | 1Gbps unmetered | 60Gbps included | [$169–189/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=487) |  |
| Intel Xeon E3-1270v2 (10Gbps unmetered) | 16 GB | 2TB HDD or 120GB SSD | 10Gbps unmetered | 60Gbps included | [$269–789/mo](https://portal.sharktech.net/aff.php?aff=1611&pid=490) |  |
| Any dedicated server + 100Gbps add-on | — | — | — | 100Gbps Anycast | +$39/IP/mo | [Add 100Gbps protection](https://bit.ly/SharKTech) |

The 100Gbps add-on is worth pulling the trigger on if you're running public-facing services that attract adversarial traffic — game servers, gambling platforms, fintech endpoints, anything where a 60Gbps ceiling feels like it's cutting it close. The Anycast spread across five data centers is what makes a single IP survive floods that would saturate any one location.

👉 [See current dedicated server inventory](https://bit.ly/SharKTech)

## Active Promo Codes

Sharktech doesn't run flash sales or manufacture FOMO with expiring coupons. Their discounts are structural, which is actually better news for anyone planning to stick around.

| Code | Discount | Applies To | Notes |
| --- | --- | --- | --- |
| Annual billing | 50% off (automatic) | Smart VPS | Applied at checkout, no code needed |
| Semi-annual billing | 35% off (automatic) | Smart VPS | Applied at checkout |
| Quarterly billing | 25% off (automatic) | Smart VPS | Applied at checkout |
| `Y5YET1Z9EK` | 10% recurring (20% for Amsterdam) | Dedicated servers, cloud services | Lifetime recurring, not a one-time discount |
| `WHTFALL` | 10% recurring, up to 33% on Cloud VDC | Cloud Virtual Servers, Bare-Metal | Recurring lifetime discount |

The recurring nature of `Y5YET1Z9EK` is the part worth paying attention to — it's not a first-month teaser, it applies every billing cycle for as long as you're a customer. Stack it with annual billing on the Smart VPS side and the math gets genuinely hard to argue with.

👉 [Apply promo codes at Sharktech checkout](https://bit.ly/SharKTech)

## Who This Is Really For

A GRE DDoS protection service is not a one-size-fits-all purchase. Based on the use cases that show up consistently in customer reviews and forum threads, Sharktech's setup tends to be a strong fit for:

- **Game server operators** who deal with regular DDoS attacks as a daily operational reality, not an edge case. Multiple game hosting companies publicly report their servers absorbing 3–8Gbps floods without interruption, and one noted attacks up to 38Gbps being handled cleanly.
- **ISPs and hosting providers** who want to offer DDoS protection to their own downstream customers without building scrubbing infrastructure from scratch. The BGP/GRE model is designed for exactly this — you announce your prefixes, Sharktech cleans the traffic, clean packets come back over the tunnel.
- **Businesses running their own infrastructure elsewhere** (a cage in a different colo, an ISP relationship they don't want to break) who just need a mitigation layer in front of it. Remote Network DDoS Protection is purpose-built for this.
- **Anyone migrating off hyperscalers** like AWS or Azure who's tired of unpredictable egress bills and surprise attack-handling fees. Sharktech's flat-rate pricing and included mitigation are a meaningful cost reduction for the right workload.

Where it's probably *not* the right fit: if you need managed WordPress hosting, click-to-deploy app environments, or a money-back guarantee to feel comfortable trying a new provider. Sharktech's services assume you know your way around a Linux command line — they're unmanaged infrastructure, and all payments are non-refundable. If that's not your comfort zone, their Cloud Application Platform handles the software layer for you, but the core hosting products expect technical competence.

## The Bottom Line

GRE-based DDoS protection works because it solves the right problem in the right place: it gets attack traffic cleaned at the network edge, before it touches your infrastructure, without forcing you to migrate or buy hardware. Sharktech has been running this playbook since 2003, their five-data-center Anycast setup gives them 1.1Tbps of pooled mitigation capacity, and their pricing is genuinely accessible — 60Gbps protection is free with every hosted service, and the 100Gbps upgrade is $39/IP/month.

If you're shopping for a GRE DDoS protection service and you want a provider whose entire identity is built around keeping servers online during attacks, this is a reasonable place to land. The annual VPS discount alone (half price, automatic) makes the entry cost hard to argue with, and the recurring promo codes stack on top for dedicated and cloud deployments.

👉 [Get started with Sharktech DDoS protection](https://bit.ly/SharKTech)
