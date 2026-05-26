# Luminaluxe — Technical Configuration

> Documenting DNS, hosting, and platform integration for luminaluxe.space

---

## 1. Hosting Architecture

| Service | Purpose | Domain |
|---------|---------|--------|
| **Vercel** | Main marketing website (landing, collections, journal, about) | `luminaluxe.space` (apex/root) |
| **Shopify** | E-commerce / online store | `shop.luminaluxe.space` (subdomain) |

---

## 2. DNS Configuration for luminaluxe.space

These records should be configured at the domain registrar's DNS management panel.

### 2.1 Root Domain → Vercel

The apex domain `luminaluxe.space` points to Vercel's global edge network.

| Type | Name | Value | TTL |
|------|------|-------|-----|
| **A** | `@` | `76.76.21.21` | Auto |
| **CNAME** | `www` | `cname.vercel-dns.com` | Auto |

> Vercel recommended setup: Use Vercel Nameservers for full control, OR add the A record above if you must keep your current nameserver.
> 
> For Vercel DNS: add the domain in Vercel dashboard → Domains → `luminaluxe.space`. Vercel will automatically verify ownership via the CNAME record or by checking the configured nameservers.

### 2.2 Shopify Subdomain

The subdomain `shop.luminaluxe.space` points to Shopify's infrastructure.

| Type | Name | Value | TTL |
|------|------|-------|-----|
| **CNAME** | `shop` | `shops.myshopify.com` | Auto |

> After adding the CNAME, go to Shopify Admin → Settings → Domains → Connect existing domain, and enter `shop.luminaluxe.space`. Shopify will verify ownership and provision an SSL certificate.

---

## 3. Verification Steps

### Vercel Apex Verification
```bash
dig +short A luminaluxe.space
# Expected: 76.76.21.21

dig +short CNAME www.luminaluxe.space
# Expected: cname.vercel-dns.com
```

### Shopify Subdomain Verification
```bash
dig +short CNAME shop.luminaluxe.space
# Expected: shops.myshopify.com
```

### SSL/TLS
- Vercel provisions automatic SSL via Let's Encrypt for `luminaluxe.space` and `www.luminaluxe.space`
- Shopify provisions automatic SSL for `shop.luminaluxe.space`
- DNS propagation typically takes 5–30 minutes (up to 48 hours for some TLDs)

---

## 4. Environment Variables

Store these in Vercel Project Settings → Environment Variables.

| Variable | Value | Scope |
|----------|-------|-------|
| `NEXT_PUBLIC_SHOPIFY_DOMAIN` | `shop.luminaluxe.space` | Production |
| `NEXT_PUBLIC_SITE_URL` | `https://luminaluxe.space` | Production |

---

## 5. Maintenance Notes

- All DNS changes should be made at the registrar level (where `luminaluxe.space` was purchased)
- Never expose Shopify API keys or storefront access tokens in client-side code
- Vercel Preview Deployments will use `.vercel.app` subdomains — update env vars accordingly for preview branches

*Last updated: May 2026*
