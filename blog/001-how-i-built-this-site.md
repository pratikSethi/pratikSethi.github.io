---
title: "How I Built This Site: GitHub Pages + Cloudflare Custom Domain"
description: "Step-by-step guide to setting up a personal blog with GitHub Pages, Cloudflare DNS, and a custom domain"
date: 2025-01-04
lastUpdated: 2025-01-04
tags: ["github-pages", "cloudflare", "dns", "ssl", "web-hosting"]
draft: true
---

# How I Built This Site: GitHub Pages + Cloudflare Custom Domain

*My second blog post - documenting the journey from zero to a live personal site.*

So I finally did it. After years of thinking "I should really have a personal site," I bought a domain and set it up. Here's exactly how I did it, including the gotchas I ran into along the way.

## The Stack

- **Domain registrar**: Cloudflare ($13/year for a .dev domain)
- **Hosting**: GitHub Pages (free)
- **SSL**: Automatic via GitHub Pages + Let's Encrypt

Total cost: ~$13/year. Not bad.

## Step 1: Buy the Domain

I'd previously used Google Domains for another project, but Google sold that business to Squarespace. Time to find a new registrar.

I've always regarded Cloudflare highly - they publish quality incident postmortems, build solid tech, and offer at-cost domain pricing (they charge exactly what the registry charges them, no markup). Since I'd be using their DNS anyway, it made sense to keep everything in one place.

Grabbed `pratiksethi.dev` and moved on.

## Step 2: Set Up DNS Records in Cloudflare

This is where it gets slightly technical. GitHub Pages needs your domain to point to their servers, and you configure that in your DNS provider - in my case, Cloudflare.

### A Records (for the root domain)

GitHub Pages uses four IP addresses for redundancy and load balancing. I added all four as A records in Cloudflare's DNS settings:

| Type | Name | Content |
|------|------|---------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

*These IPs come from [GitHub's official documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site). They're GitHub-specific — don't use them for other hosting providers.*

### CNAME Record (for www)

| Type | Name | Target |
|------|------|--------|
| CNAME | www | pratiksethi.github.io |

Note: Use `pratiksethi.github.io`, not `www.pratiksethi.github.io`. GitHub doesn't create a www subdomain on their .github.io domain.

### Why not a CNAME for the root domain?

I wondered this too. Turns out you can't (well, shouldn't) have a CNAME at the apex/root domain — it violates DNS standards and can break other records like MX and TXT. A records are the correct approach for root domains.

### Email Anti-Spoofing Records

Since I'm not using email on this domain (yet), Cloudflare recommended adding restrictive SPF, DKIM, and DMARC records. I'll be honest - I didn't fully dig into what each of these does. But the gist is: they tell email servers "this domain doesn't send email so reject anything claiming to be from it."

| Type | Name | Content |
|------|------|---------|
| TXT | @ | `v=spf1 -all` |
| TXT | _dmarc | `v=DMARC1; p=reject; sp=reject; adkim=s; aspf=s;` |
| TXT | *._domainkey | `v=DKIM1; p=` |

This prevents spammers from spoofing my domain. I can always swap these out for real email records later if I want `me@pratiksethi.dev`. Understanding email authentication is on my list of things to learn properly - maybe a future blog post.

### Important: Proxy Status

Cloudflare has a "proxy" feature (orange cloud) that routes traffic through their CDN. **Turn this off** (gray cloud / "DNS only") for all records until GitHub successfully issues your SSL certificate. The proxy can interfere with certificate verification.

## Step 3: Configure GitHub Pages

1. Created a repo named `pratiksethi.github.io`
2. Went to **Settings → Pages**
3. Entered `pratiksethi.dev` as the custom domain
4. Waited for DNS verification (green checkmark)

## Step 4: Wait for SSL Certificate

This is where I hit my first snag. After setting up the custom domain, the "Enforce HTTPS" checkbox was grayed out with a message about the domain not being properly configured.

Turns out GitHub was still provisioning the TLS certificate via Let's Encrypt. The progress indicator showed "1 of 3" steps.

**The fix**: Just wait. It took about 15 minutes for the certificate to be issued. If you're stuck longer than an hour, double-check that Cloudflare's proxy is disabled (gray cloud).

I also had to remove and re-add the custom domain in GitHub settings after switching from proxied to DNS-only in Cloudflare - this forced GitHub to retry the certificate issuance.

## Step 5: The CNAME File

One thing that tripped me up: GitHub Pages needs a `CNAME` file in your repo root containing your custom domain. GitHub creates this automatically when you set the custom domain in settings, but if you're working across branches or reset things, it might go missing.

The file is literally just:
```
pratiksethi.dev
```

That's it. One line, no quotes, no nothing.

## Step 6: Actually Add Content

After all that DNS and certificate work, I visited my site and... nothing. Blank page.

Right. I needed actual content. GitHub Pages serves `index.html` (or `index.md` if you're using Jekyll) from your repo root. I threw up a simple "Coming Soon" page and pushed it.

Finally, `https://pratiksethi.dev` showed something.

## Lessons Learned

1. **DNS propagation is fast** — Cloudflare's DNS updates were nearly instant. The waiting was mostly for GitHub's certificate provisioning.

2. **Disable Cloudflare proxy initially** — Let GitHub handle SSL first. You can enable the proxy later for CDN benefits.

3. **The CNAME file matters** - If your custom domain stops working after branch changes, check if the CNAME file is still there.

4. **Email records are optional but smart** - If you're not using email, add the anti-spoofing records. Takes 2 minutes and prevents your domain from being used in spam.

5. **GitHub's IPs are stable** - Those four IP addresses have been the same for years, but always verify against the official docs if you're troubleshooting.

## What's Next

This "Coming Soon" page is just the beginning. I'm planning to:
- Set up a proper static site generator (probably Hugo or Astro)
- Write about things I'm learning and building
- Maybe add some project showcases

But for now, I'm just happy the domain works. Sometimes shipping something small is better than planning something perfect.

---

*Published on pratiksethi.dev - a site that now actually exists.*
