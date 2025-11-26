# DNS Resolver Theory

A DNS resolver is the component that turns a human-friendly domain name like `example.com` into the IP address that machines actually use to communicate. It sits between your application (browser, CLI tool, etc.) and the global DNS system, handling all the steps needed to answer “what is the IP for this name?”.

## High-level idea

When you enter a domain in your browser, your device does not directly talk to every DNS server on the planet. Instead, it sends one query to a resolver. The resolver then walks through the DNS hierarchy (root, TLD, authoritative servers) and returns the final answer, caching results so repeated lookups are fast.

You can think of the resolver as a smart DNS client that knows how to navigate the tree of DNS servers on behalf of many users.

## Steps of DNS resolution

1. **Application query**  
   An application (like a browser) calls the OS resolver API to resolve a hostname into an IP address. The OS then forwards this request to a configured DNS resolver (often your ISP, a local resolver on your network, or a public resolver).

2. **Check local cache**  
   Before asking external servers, the resolver checks its own cache to see if it already knows the answer for that domain and record type (for example `A` or `AAAA`). If a fresh cached entry exists, it returns the answer immediately.

3. **Contact a root server**  
   If the resolver has no cached answer, it sends a query to one of the DNS root servers. Root servers do not know the final IP, but they know which Top-Level Domain (TLD) name servers are responsible for `.com`, `.org`, `.net`, and so on.

4. **Contact a TLD server**  
   The root server replies with a referral: a list of name servers for the TLD of the domain (for example, the `.com` TLD servers for `example.com`).  
   The resolver then sends the query to one of these TLD servers.

5. **Contact the authoritative server**  
   The TLD server does not usually know the final IP either. It replies with another referral: the authoritative name servers for the specific domain (for example, `ns1.example-dns.com` for `example.com`).  
   The resolver then queries one of these authoritative servers.

6. **Receive the final answer**  
   The authoritative server returns the actual resource records: for example an `A` record with an IPv4 address, or an `AAAA` record with an IPv6 address.  
   The resolver returns this answer to the original client and stores it in its cache for future use.

From the user’s perspective, this entire chain usually completes in milliseconds.

## Caching and TTL

DNS caching is critical for performance and for reducing load on the global DNS infrastructure.

- **Cache entries**  
  When the resolver gets an answer from an authoritative server, it stores the result along with its Time To Live (TTL) value.  
- **TTL**  
  TTL is a number of seconds that tells the resolver how long the record should be considered valid. As long as the TTL has not expired, the resolver can answer new queries from its cache instead of repeating the full lookup.
- **Expiration**  
  Once TTL expires, the resolver discards the cached entry or treats it as stale and must query upstream servers again to refresh it.

Good caching behavior dramatically reduces latency for frequent domains and protects authoritative servers from unnecessary load.

## Recursive vs iterative behavior

Two important concepts in resolver behavior are **recursive** and **iterative** queries:

- **Recursive query (from client to resolver)**  
  A client typically sends a recursive query to the resolver, which means: “either give me the final answer or an error; do all the work for me.” The resolver then takes responsibility for walking the DNS hierarchy.

- **Iterative queries (between resolvers and DNS servers)**  
  When the resolver talks to root, TLD, and authoritative servers, it usually performs iterative queries. Each server responds with the best information it has (often a referral), and the resolver decides which server to ask next.

This pattern lets simple clients stay dumb while resolvers implement the full logic for navigating DNS.

## Types of DNS records a resolver handles

A resolver is not limited to domain→IP lookups. It can resolve various DNS record types, including:

- **A / AAAA** – IPv4 and IPv6 addresses for hostnames.  
- **CNAME** – alias from one name to another hostname.  
- **MX** – mail exchanger records, used for email delivery.  
- **TXT** – arbitrary text records, often used for verification and policies (SPF, DKIM, etc.).  
- **NS** – name server records that indicate which servers are authoritative for a zone.

The resolver follows CNAME chains if necessary and combines multiple records to build a complete picture for the client.

## Failure modes and errors

Resolvers must also handle error cases gracefully:

- **Non-existent domain (`NXDOMAIN`)**  
  The authoritative server indicates that the requested name does not exist in the zone. The resolver returns an error to the client and may cache this negative answer for a shorter TTL.

- **No data for that record type**  
  The name exists, but there is no record of the requested type (for example, an `MX` query for a host that only has `A` records). The resolver returns a “no data” response.

- **Timeouts and retry**  
  If a DNS server does not respond in time, the resolver retries with other servers or returns a temporary failure to the client.

Robust resolvers implement sensible timeouts, retry logic, and negative caching to remain reliable under failure.

## Security considerations

DNS was not originally designed with strong security, so modern resolvers add protections:

- **DNSSEC validation**  
  By validating DNSSEC signatures, resolvers can detect tampered or spoofed DNS responses and protect clients from certain attacks.

- **Rate limiting and filtering**  
  Resolvers can rate-limit abusive clients, block malicious domains, or apply policies such as parental controls or ad-blocking.

- **Anycast and redundancy**  
  Large public resolvers deploy many instances using anycast addresses, so queries automatically go to the nearest healthy node.

These measures keep resolution both fast and safer for end users.

## Summary

A DNS resolver is the “DNS brain” between your application and the vast network of DNS servers. It handles caches, walks the root→TLD→authoritative hierarchy, validates answers, and returns a clean result to the client. Understanding the resolver’s role helps when debugging slow lookups, designing your own resolver, or tuning DNS for performance and reliability.
