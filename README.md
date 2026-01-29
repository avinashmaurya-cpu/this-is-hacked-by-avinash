# Subdomain Takeover (Dangling CNAME)

## Name of the attack

Subdomain Takeover (dangling CNAME / DNS takeover)

## Description

A subdomain takeover occurs when a DNS record (commonly a CNAME) for a subdomain points to a third-party hosting service (e.g., cloud storage, CDN, PaaS) but the resource at the provider has not been claimed or has been removed. An attacker can register or claim the unclaimed resource at the provider and serve content from the victim's subdomain.

## Impact

- Attacker-controlled content served from a trusted subdomain
- Phishing pages hosted on the legitimate domain
- Cookie theft or session hijacking (if cookies are not scoped securely)
- Distribution of malware or malicious JavaScript
- Reputation damage and loss of user trust
- Possible bypasses of same-origin protections for subdomains depending on configuration

## Severity

High — The severity depends on the privileges associated with the affected subdomain. A takeover of an administrative or authentication-related subdomain (e.g., login.example.com, admin.example.com) is critical and could lead to account compromise or full application takeover.

## Steps to reproduce (high-level, for testing and remediation)

1. Inventory subdomains and their DNS records (focus on CNAME records pointing to external providers).
2. Identify CNAMEs that point to third-party services (e.g., cloud storage buckets, GitHub Pages, Heroku, Azure, AWS S3, Fastly, etc.).
3. Verify whether the target resource exists or is claimed at the provider. If the provider returns an indication that the resource is unclaimed (provider-specific 404 or "no such site" response), the subdomain may be vulnerable.
4. Confirm the impact in a controlled, authorized environment (do NOT claim third-party resources on systems you do not own or have explicit permission to test). Instead, coordinate with the service owner or follow an approved disclosure/testing plan.

Note: The above is a high-level testing workflow intended for owners and authorized testers. Do not use these techniques for unauthorized access or exploitation.

## Remediation

- Remove unnecessary DNS records that point to external providers.
- If a DNS entry is required, ensure the target resource is properly claimed and configured at the provider.
- Enforce least-privilege and minimize the number of subdomains exposed publicly.
- Monitor DNS changes and set up alerts for newly added CNAME records.
- Restrict cookies and sensitive data to appropriate hostnames and set the Secure and HttpOnly flags.
- Use HTTPS with HSTS where applicable and ensure certificates are issued only for intended hostnames.
- For long-term protection, adopt DNS-management practices that require change approval and track ownership of external resources.

## References

- PortSwigger: Subdomain takeover — https://portswigger.net/web-security/subdomain-takeover
- OWASP community page: Subdomain takeover — https://owasp.org/www-community/attacks/Subdomain_takeover