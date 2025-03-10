---
pcx_content_type: faq
source: https://support.cloudflare.com/hc/en-us/articles/360017421192-Cloudflare-DNS-FAQ
title: General FAQ
weight: 1
---

# General FAQ

## Is Cloudflare a free DNS (domain nameserver) provider?

Yes. Cloudflare offers [free DNS services](https://www.cloudflare.com/dns) to customers in all plans. Note that:

1.  You do not need to change your hosting provider to use Cloudflare.
2.  You do not need to move away from your registrar. The only change you make with your registrar is to point the authoritative nameservers to the Cloudflare nameservers.

___

## Does Cloudflare charge for or limit DNS queries?

Cloudflare never limits or caps DNS queries, but the pricing depends on your plan level.

For customers on Free, Pro, or Business plans, Cloudflare does not charge for DNS queries.

For customers on Enterprise plans, Cloudflare uses the number of monthly DNS queries as a pricing input to generate a custom quote.

___

## Where do I change my nameservers to point to Cloudflare?

Make the change at your registrar, which may or may not be your hosting provider. If you don't know who your registrar is for the domain, you can find this by doing a [WHOis search](http://www.whois.net/). Follow the instructions in [change nameservers to Cloudflare](/dns/zone-setups/full-setup/setup/).

___

## Does Cloudflare limit number of DNS records a domain can have?

Yes. Currently Free, Pro, and Business customers have a limit on the number of DNS records they can create.

If you are an Enterprise customer you can contact your Account team if you require more DNS records.

___

## Which record types does Cloudflare not proxy?

Cloudflare does not proxy the following record types:

-   `LOC`
-   `MX`
-   `NS`
-   `SPF`
-   `TXT`
-   `SRV`
-   `CAA`

___

## Can I CNAME a domain not on Cloudflare to a domain that is on Cloudflare?

No. If you would like to do a redirect for a site not on Cloudflare, then set up a traditional `301` or `302` redirect on your origin web server.

Redirecting non-Cloudflare sites via `CNAME` records would cause a DNS resolution error. Since Cloudflare is a reverse proxy for the domain that is on Cloudflare, the `CNAME` redirect for the domain (not on Cloudflare) would not know where to send the traffic to.

___

## Does Cloudflare support wildcard DNS entries?

Cloudflare supports proxying wildcard '\*' record for DNS management in all customer plans.

___

## How long does it take for a DNS change I made to push out?

By default, any changes or additions you make to your Cloudflare zone file will push out in 5 minutes or less. Your local DNS cache may take longer to update; as such, propagation everywhere might take longer than 5 minutes.

This setting is controlled by the Time-to-Live (TTL) value on a [DNS record](/dns/manage-dns-records/how-to/create-dns-records/). Proxied records update within 300 seconds (Auto), but the TTL for unproxied records can be customized.

___

## Does Cloudflare offer domain masking?

No. Cloudflare does not offer domain masking or DNS redirect services (your hosting provider might). However, we do offer URL forwarding through [Bulk Redirects](/rules/url-forwarding/bulk-redirects/).

___

## Why can't I make ANY queries to Cloudflare DNS servers?

`ANY` queries are special and often misunderstood. They are usually used to get all record types available on a DNS name, but what they return is just any type in the cache of recursive resolvers. This can cause confusion when they are used for debugging.

Because of Cloudflare's many advanced DNS features like CNAME flattening, it can be complex and even impossible to give correct answers to `ANY` queries. For example, when DNS records dynamically come and go or are stored remotely, it can be taxing or even impossible to get all the results at the same time.

`ANY` is rarely used in production, but is often used in DNS reflection attacks, taking advantage of the lengthy answer returned by `ANY`.

Instead of using `ANY` queries to list records, Cloudflare customers can get a better overview of their DNS records by logging in and checking their DNS app settings.

The decision to block `ANY` queries was implemented for all Authoritative DNS customers in September 2015, and does not affect Virtual DNS customers.

Read [Deprecating the DNS ANY meta-query type](https://blog.cloudflare.com/deprecating-dns-any-meta-query-type/) in the Cloudflare blog.

___

## Why do I have to remove my `DS` record when signing up for Cloudflare?

{{<render file="_dnssec-providers.md">}}

For more help, refer to [Enabling DNSSEC in Cloudflare](/dns/dnssec/).

___

## What happens when I remove the `DS` record?

When you remove your DS record, an invalidation process begins which results in the unsigning of your domain’s DNS records. This will allow your authoritative nameservers to be changed. If you are an existing customer, this will not affect your ability to use Cloudflare. New customers will need to complete this step before Cloudflare can be used successfully.

___

## Does Cloudflare support EDNS0 (extension mechanisms for DNS)?

Yes, Cloudflare DNS supports EDNS0. EDNS0 is enabled for all Cloudflare customers. It is a building block for modern DNS implementations that adds support for signaling if the DNS Resolver (recursive DNS provider) supports larger message sizes and DNSSEC.

EDNS0 is the first approved set of mechanisms for [DNS extensions](http://en.wikipedia.org/wiki/Extension_mechanisms_for_DNS), originally published as [RFC 2671](https://datatracker.ietf.org/doc/html/rfc2671).

___

## What should I do if I change my server IP address or hosting provider?

After switching hosting providers or server IP addresses, update the IP addresses in your Cloudflare **DNS** app. Your new hosting provider will provide the new IP addresses that your DNS should use.  To modify DNS record content in the **DNS** app, click on the IP address, and enter the new IP address.

___

## Where can I find my Cloudflare nameservers?

Under the **DNS** app of your Cloudflare account, review the **Cloudflare Nameservers**.

The IP address associated with a specific Cloudflare nameserver can be retrieved via a dig command or a third-party DNS lookup tool hosted online such as [whatsmydns.net](https://www.whatsmydns.net/):

```sh
$ dig kate.ns.cloudflare.com
kate.ns.cloudflare.com.    68675    IN    A    173.245.58.124.
```

___

## Why are Cloudflare's A or AAAA records / IP addresses for my domain's DNS responses appearing?

For DNS records proxied to Cloudflare, Cloudflare's IP addresses are returned in DNS queries instead of your original server IP address. This allows Cloudflare to optimize, cache, and protect all requests for your website.

___

## Should the cloud icon beside my DNS record be orange or gray?

By default, only A and CNAME records that handle web traffic (HTTP and HTTPS) can be proxied to Cloudflare. All other DNS records should be toggled to a gray cloud. For further details, refer to our [support guide](/dns/manage-dns-records/reference/proxied-dns-records).

___

## Can subdomains be added directly to Cloudflare?

Only Enterprise customers can add subdomains directly to Cloudflare via [Subdomain Support](/dns/zone-setups/subdomain-setup/).

___

## 403 Authentication error when creating DNS records using Terraform

**Problem Description**

`Error: failed to create DNS record: HTTP status 403: Authentication error (10000)` is returned when using Terraform with Cloudflare API.

**Root Cause**

Error seems to be misleading, as the error was found to be in customer code syntax, specifically: zone\_id = data.cloudflare\_zones.example\_com.id

**Solution**

Make sure the argument `zone_id = data.cloudflare_zones.example_com.zones[0].id`. A more detailed use case can be found in [this](https://github.com/cloudflare/terraform-provider-cloudflare/issues/913) GitHub thread.

___

## Why am I getting hundreds of random DNS records after adding my domain?

This can happen when you had a wildcard \* record configured at your previous authoritative DNS. You can remove these records in bulk [using the API](/api/operations/dns-records-for-a-zone-delete-dns-record).

You can also:
1. [Remove your domain](/fundamentals/setup/manage-domains/remove-domain/) from Cloudflare.
2. Delete the wildcard record from your authoritative DNS.
3. [Re-add](/fundamentals/setup/account-setup/add-site/) the domain.

___

## What IP should I use for parked domain / redirect-only / originless setup?

In the case a placeholder address is needed for “originless” setups, use the IPv6 reserved address `100::` or the IPv4 reserved address `192.0.2.0` in your Cloudflare DNS to create a proxied DNS record that can use Cloudflare Page Rules or Cloudflare Workers.

## Why are DNS queries returning incorrect results?

Third-party tools can sometimes fail to return correct DNS results if a recursive DNS cache fails to refresh. In this circumstance, purge your public DNS cache via these methods:

-   [Purging your DNS cache at OpenDNS](http://www.opendns.com/support/cache/)
-   [Purging your DNS cache at Google](https://developers.google.com/speed/public-dns/cache)
-   [Purging your DNS cache locally](https://documentation.cpanel.net/display/CKB/How%2BTo%2BClear%2BYour%2BDNS%2BCache)

___

## No A, AAAA or CNAME record found?

`No A, AAAA or CNAME record found` means the Cloudflare **DNS** app lacks proper records for DNS resolution.

[Add the missing DNS records](/dns/manage-dns-records/how-to/create-dns-records) to your domain.

{{<Aside type="note">}}
Sites generally have at least an `A` record that points to the origin
server IP address, typically for the `www` subdomain and the apex domain (also known as "root domain" and represented by `@`).
{{</Aside>}}

___

## Why have I received an email: Your Name Servers have Changed?

For domains where Cloudflare hosts the DNS, Cloudflare continuously checks whether the domain uses Cloudflare’s nameservers for DNS resolution. If Cloudflare's nameservers are not used, the [domain status](/dns/zone-setups/reference/domain-status/) is updated from _Active_ to _Moved_ in the Cloudflare **Overview** app and an email is sent to the customer.

This is important because - if a domain is in a *Moved* state for a [long enough period of time](/dns/zone-setups/reference/domain-status/) - it will be deleted from Cloudflare.

{{<render file="_recover-deleted-domain.md">}}

___

## Why can't I add certain TLDs via the DNS API?

The DNS API cannot be used for domains with `.cf`, `.ga`, `.gq`, `.ml`, or `.tk` TLDs. Use the Cloudflare Dashboard for managing such TLDs.

Enterprise customer can [contact Cloudflare Support](/support/troubleshooting/general-troubleshooting/contacting-cloudflare-support/) to remove this limitation.