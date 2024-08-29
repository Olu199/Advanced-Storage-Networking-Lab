1. **Person Registering the Domain**: 
   - The user decides to register a domain name (e.g., `cats.com`), assuming it is available.

2. **Domain Registrar**:
   - The Domain Registrar is the service where you purchase the domain name, such as Google Domains, AWS Route 53 Registered Domains, or Hover.
   - The Registrar has established relationships with the Top-Level Domain (TLD) Registries, which manage the domain extensions (e.g., `.com`).

3. **DNS Hosting Provider**:
   - This is the service that operates the DNS name servers where DNS zones are hosted. 
   - Some companies can act as both the Domain Registrar and DNS Hosting Provider, such as AWS Route 53 and Hover.
   - If the Registrar and DNS Hosting Provider are different, after purchasing the domain, you will need to create a DNS zone separately. The Registrar will ask for the name server (NS) information where this zone will be hosted.

4. **TLD Registry**:
   - The TLD Registry (e.g., Verisign for `.com` domains) is responsible for maintaining the global DNS for specific TLDs.
   - Once you register your domain and provide the NS information, the TLD Registry adds this domain’s NS information to the TLD Zone.

5. **TLD Zone**:
   - This is the DNS zone at the TLD level, which contains information about which name servers are authoritative for domains under that TLD (e.g., `.com`).

6. **Domain Becomes Live**:
   - After the newly purchased domain name has been added to a DNS zone and the name server information has been correctly configured, the TLD Registry updates its TLD Zone.
   - Once updated, the domain (`cats.com`) becomes live and accessible on the internet.
