# DMIT EPYC 9654 VPS Complete Guide: Which Plans Run on AMD Genoa Hardware, Real Benchmark Numbers, and How to Pick Between CN2 GIA vs CMIN2+9929 Routing (With Full Plan Comparison Table)

If you've been digging around for a US-based VPS with serious China-optimized routing, you've probably hit the "DMIT EPYC 9654" combination somewhere in your research. Maybe in a benchmark screenshot, maybe in someone's traceroute thread. The two names keep showing up together for a reason.

Here's the short version: DMIT's AN4 hardware platform runs on AMD EPYC 9654 processors, and it's what's powering a good chunk of their Los Angeles lineup right now—including some of their best-value plans. This guide covers what that actually means for your workload, which plans carry this CPU, what the network routing looks like for China access, and how to pick the right tier without overspending.

---

## What Is the AMD EPYC 9654, and Why Does It Matter for a VPS?

The EPYC 9654 is AMD's 96-core Genoa processor—the "9004 series" in AMD's naming scheme, one generation before Turin (9005 series). At 2.4 GHz base clock with a 380MB L3 cache, it's the kind of hardware that doesn't show up in budget hosting at all. DMIT deploying it across their entire AN4 platform—including plans priced under $50/year—is genuinely unusual.

For day-to-day VPS use, the EPYC 9654 shows up in three ways:

1. **Single-core speed matters for most tasks.** Web servers, databases, small APIs—these live and die on single-threaded performance. The Genoa architecture holds up well here. I compiled a mid-size project on a 1-vCPU instance running the 9654 and the responsiveness felt more like a local dev box than a typical cloud node.
2. **Disk I/O runs at proper NVMe speeds.** Sequential reads and writes on DMIT's AN4 nodes land around 1 GB/s consistently. Random 4K IOPS sits around 15,000+. That's the difference between a database that queries fast and one that makes you stare at a loading spinner.
3. **No lottery on hardware.** Before DMIT unified on the 9654, you could end up on a 7402 or 7443P depending on which node you landed on. Now the AN4 platform is standardized—same chip, every node.

DMIT has since launched the AN5 platform using the newer EPYC 9005 series (Turin, higher performance) for their flagship plans. The AN4 / EPYC 9654 platform remains active across most of their current special-offer and standard plans. If you're buying one of the plans below, that's the processor you're getting.

---

## DMIT's LA Plans: Which Ones Run on EPYC 9654

DMIT structures their LA offerings across three product lines, each with different routing profiles:

- **Pro (Premium)** — CN2 GIA tri-network optimization. Best for latency-sensitive China access.
- **Eyeball (EB)** — CMIN2 + AS9929 return routing. Lower cost, still meaningfully optimized.
- **Tier 1 (T1)** — Standard international routing. No China optimization.

Plans on the AN4 platform (EPYC 9654) include most of the currently available Eyeball series and several special-offer plans. The AN5 platform (EPYC 9005) has rolled out for the flagship Pro and EB TINY and above regular plans, while AN4-based plans continue shipping for budget and limited-run options.

### Full Plan Comparison Table

| Plan | CPU Platform | vCPU | RAM | Storage | Bandwidth | Monthly Traffic | Routing | Network | Price | Link |
|------|-------------|------|-----|---------|-----------|-----------------|---------|---------|-------|------|
| LAX.AN4.EB.WEE | EPYC 9654 | 1 | 1 GB | 20 GB NVMe | 1 Gbps | 1,000 GB | CMIN2 + AS9929 | IPv4 + IPv6/64 | $39.9/yr |  [View Plan](https://www.dmit.io/aff.php?aff=13832) |
| LAX.EB.CORONA | EPYC 9654 | 1 | 1 GB | 20 GB NVMe | 2 Gbps | 2,000 GB | CMIN2 + AS9929 | IPv4 + IPv6/64 | $49.9/yr |  [View Plan](https://www.dmit.io/aff.php?aff=13832&pid=218) |
| LAX.EB.FONTANA | EPYC 9654 | 2 | 2 GB | 40 GB NVMe | 4 Gbps | 4,000 GB | CMIN2 + AS9929 | IPv4 + IPv6/64 | $100/yr |  [View Plan](https://www.dmit.io/aff.php?aff=13832&pid=219) |
| LAX.AN5.EB.TINY | EPYC 9005 | 1 | 2 GB | 20 GB NVMe | 1 Gbps | 1,200 GB | CMIN2 + AS9929 | IPv4 + IPv6/64 | Check site |  [View Plan](https://www.dmit.io/aff.php?aff=13832) |
| LAX.AN5.Pro.TINY | EPYC 9005 | 1 | 2 GB | 20 GB NVMe | 1 Gbps | 1,000 GB | CN2 GIA | IPv4 + IPv6/64 | ~$9.9/mo |  [View Plan](https://www.dmit.io/aff.php?aff=13832) |
| LAX.AN5.Pro.STARTER | EPYC 9005 | 2 | 2 GB | 40 GB NVMe | 2 Gbps | 2,000 GB | CN2 GIA | IPv4 + IPv6/64 | Check site |  [View Plan](https://www.dmit.io/aff.php?aff=13832) |

> **Note:** Pricing and availability change frequently—DMIT's popular plans sell out during restocks. Check current availability and exact pricing directly before buying. Plans marked "Check site" should be verified at purchase time.

👉 [See all DMIT plans and current availability](https://www.dmit.io/aff.php?aff=13832)

---

## The Network Routing Question: CMIN2+9929 vs CN2 GIA

This is actually the decision that matters more than anything else, including the CPU.

DMIT operates two distinct China-optimized routing profiles on their LA nodes, and they genuinely serve different use cases.

**CN2 GIA (Pro series)**

The full name is China Telecom's Global Internet Access network. All three major Chinese carriers—Telecom, Unicom, and Mobile—get GIA return routing. In practice this means latency to major Chinese cities sits around 150-165ms and stays there during evening peak hours (8–11 PM Beijing time) when most providers start degrading.

This is the one you want if: you're building something that Chinese users actively interact with in real time. API calls, live websites, anything where every 30ms of extra latency is user-visible. The price is real—it's significantly more expensive than the Eyeball tier for the same hardware specs.

**CMIN2 + AS9929 (Eyeball series)**

The Eyeball series launched in 2024 with a different pitch: good enough for most use cases at meaningfully lower prices. The return routing works like this:
- China Mobile → CMIN2 (premium mobile network)
- China Unicom → AS9929 (high-quality Unicom network)
- China Telecom → load-balanced between AS9929 and CMIN2

Latency runs 128–136ms to major cities in good conditions. During peak hours, it holds up well—the CMIN2 and 9929 routes are both quality-tier, not standard BGP. The main difference from CN2 GIA is that Telecom users don't get the absolute best routing path. For most users, that's an acceptable tradeoff.

I've run both long-term. Honestly, if your primary users are on China Mobile or Unicom, the Eyeball tier is hard to argue against at the price difference. It only becomes a clear CN2 GIA situation when you need Telecom stability at peak hours, or when sub-150ms latency is a hard requirement.

---

## Real Benchmark Numbers on the EPYC 9654 Nodes

These come from published test runs on the AN4 platform:

**CPU:** AMD EPYC 9654, 96-core, ~2.4 GHz per vCPU assigned  
**Disk I/O:**
- 4K block read/write: ~62 MB/s (15,600 IOPS)
- 64K sequential: ~1.07 GB/s read and write
- 512K sequential: ~1.02 GB/s read, ~1.07 GB/s write
- Average I/O across run modes: 666–839 MB/s

For a VPS you're running a database or file-heavy app on, those 64K/512K numbers are what you'll actually feel. Queries come back fast. The 4K random IOPS at 15K+ handles typical web application I/O without being a bottleneck.

**Memory:** DDR4 with balloon driver support. Actual usable RAM on a 1 GB plan runs around 958 MB after OS overhead.

**Virtualization:** KVM, standard PC (Q35 + ICH9). No nested virtualization on most nodes—VM-x disabled is the norm. If you need nested VM support, check before ordering.

**Network speed (local):** Speedtest.net from the node measures around 500 Mbps up/down. Real throughput to China on CMIN2 routes during off-peak hits 300–475 Mbps. During evening congestion you're looking at 60–80% of stated port speed, which tracks with DMIT's disclosed bandwidth sharing policy.

---

## Traffic Policy: What Happens When You Hit Your Monthly Limit

This changed in early 2025. DMIT now throttles instead of cutting you off when you exceed your monthly allocation.

Specifically:
- CORONA, WEE, and MALIBU-class plans: throttle to 2 Mbps after quota
- TINY, FONTANA, STARTER-class plans: throttle to 4 Mbps
- MINI, MICRO-class plans: throttle to 8 Mbps
- MEDIUM, LARGE, GIANT-class plans: throttle to 10 Mbps

At 2 Mbps you can still SSH in, check logs, run lightweight tasks. At 4 Mbps it's usable for low-traffic web serving. It's not "your server goes dark on the 20th of the month" anymore—which was the old behavior and something people complained about.

If you tend to run heavy, factor this into plan selection. The CORONA plan at $49.9/year gives you 2 TB at 2 Gbps with a 2 Mbps fallback. For most personal projects and light business use, that's plenty.

---

## Free IP Replacement Policy

DMIT includes one free IP address replacement every 15 days. If your IP gets blocked by the Great Firewall shortly after deployment—which does happen occasionally with fresh IPs—you can swap it out without paying. This is one of the things that actually matters in practice for China-access VPS use, and DMIT makes it a standard part of the offering rather than a paid add-on.

The refund policy: 3-day full refund available on most plans (within 30 GB usage limit), with prorated refunds available up to 30 days. Not "money-back no questions asked" territory, but enough runway to verify the routing and IP quality from your actual location before fully committing.

---

## AN4 (EPYC 9654) vs AN5 (EPYC 9005): Do You Need to Upgrade?

Here's the thing most people overthink. The EPYC 9654 is not a slow CPU. It's a Genoa-architecture server chip that was state-of-the-art less than two years ago. The AN5 / EPYC 9005 Turin-generation chips are faster—particularly in multi-core workloads and memory bandwidth—but for the vast majority of VPS use cases (web hosting, proxies, development environments, API backends), the 9654 is not a limiter.

Where AN5 makes a real difference: heavy compilation jobs, multi-threaded processing, anything that actually saturates CPU cores. If you're spinning up a CI/CD runner that needs to compile large codebases quickly, the AN5 plans are worth the price delta. For everything else, the AN4 plans at their current pricing are the smarter value.

The CORONA at $49.9/year is particularly hard to beat for the Eyeball use case. Under $5/month for a real Genoa chip with 2 TB monthly traffic on CMIN2+9929 routing—there's genuinely nothing comparable in the market at this configuration level.

---

## Who Should Pick What

**Budget build/China-facing personal project** → LAX.AN4.EB.WEE or LAX.EB.CORONA. Under $5/month. CMIN2+9929 routing. Enough for a blog, a small app, a proxy node. The WEE is the cheapest; the CORONA gives you double the traffic and double the port speed.

**Business site serving Chinese users** → LAX.EB.FONTANA at $100/year, or step up to a Pro plan. At 2 vCPU / 2 GB / 4 TB traffic with 4 Gbps port, FONTANA handles real traffic volumes. For Telecom users who need GIA-level routing, the Pro series is the correct choice despite the price.

**Development environment or API backend** → Any Eyeball plan at the right size. The EPYC 9654 handles compilation and dev workloads fine. Eyeball routing won't cause issues for outbound connections or API calls; it only matters for inbound latency from China.

**High-sensitivity latency requirement (gaming, live services)** → Pro series, CN2 GIA routing. No substitute here.

👉 [Compare all DMIT plans and check availability](https://www.dmit.io/aff.php?aff=13832)

---

## How to Buy: The Short Version

1. Go to DMIT's order page via the link above
2. Select your location (Los Angeles) and product line (Eyeball for CMIN2, Pro for CN2 GIA)
3. Choose billing cycle—quarterly or longer to qualify for most discount codes
4. If you're on the Eyeball series and paying quarterly or annually, the code **LAX-EB-LAUNCH-NON-MONTHLY-RECURRING-20OFF** gives a 20% recurring discount on TINY-tier and above. That's a permanent reduction, not a one-time deal.
5. Check the plan is in stock before entering payment—DMIT doesn't oversell, which means popular plans actually run out

Payment methods: PayPal, Alipay, credit card, select cryptocurrencies.

---

## FAQ

**Q: Does DMIT's EPYC 9654 platform support IPv6?**  
All current LAX plans include dual-stack IPv4 + IPv6/64. IPv6 on the Eyeball series follows the same 9929/CMIN2 optimized routing as IPv4—it's not a degraded path.

**Q: Can I get CN2 GIA on the EPYC 9654 platform?**  
The Pro series (CN2 GIA) has started migrating to AN5 (EPYC 9005) for standard plans. Some EPYC 9654 AN4 nodes may still be running Pro plans in inventory. Check the shopping cart at purchase time—the product page shows which hardware platform a plan runs on.

**Q: How often does DMIT restock sold-out plans?**  
Restocks are irregular. The Eyeball series restocked significantly in April 2025; before that there was a gap of several months. If a specific plan is out of stock, the safest move is to bookmark the plan page and check back periodically, or go with an alternative tier that's currently available.

**Q: Is the 3-day refund genuinely available?**  
Yes, on most standard plans. The conditions: within 3 days of purchase, under 30 GB data used, and the assigned IP must remain available in its region. Refunds are prorated for longer-term plans after day 3, up to day 30. Read the specific plan's terms on the product page before ordering.

**Q: What streaming services work on DMIT LA nodes?**  
The Eyeball plans with native LA IPs tend to unlock major US streaming platforms—Amazon Prime Video US, YouTube Premium, Disney+ works on some nodes. Netflix unlocks for originals only (not full catalog) on most DMIT LA IPs. The Pro series performs similarly. This is LA native IP behavior, not specific to the EPYC 9654 platform.

**Q: Will my existing EPYC 9654 plan be automatically upgraded to AN5?**  
DMIT has stated that existing customers on regular (non-special-offer) plans will receive phased hardware upgrades to AN5 without additional cost. Special promo plans like WEE, PalmSpring, and Irvine are explicitly excluded from the upgrade path and will remain on AN4.

---

Three-day refund coverage plus the free IP swap policy gives you a reasonable window to verify the routing from your actual location. The EPYC 9654 hardware doesn't need justifying at this price point—the question is just CN2 GIA or CMIN2+9929, and that depends entirely on your users' ISP distribution.

👉 [Get started with DMIT—view current plans and pricing](https://www.dmit.io/aff.php?aff=13832)
