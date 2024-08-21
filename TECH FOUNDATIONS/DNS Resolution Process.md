### Checking `google.com`

1. **Local Cache Check:**
   - **Host File:** 
     - The first place your system checks is the local host file. This file may contain mappings of domain names to IP addresses that are manually configured.
   - **Application Cache:** 
     - The application attempting to resolve `google.com` also checks its own cache to see if the IP address has been resolved recently.
   
2. **DNS Resolver Cache Check:**
   - If the IP address is not found in the local host file or the application's cache, the system queries the DNS resolver. The resolver first checks its own cache to see if it has a recent record of `google.com`.

3. **Query to the Root Zone:**
   - If the resolver doesn't have the record cached, it will send a query to a root server. 
   - **Root Zone Servers:** 
     - The root servers are critical components in the DNS hierarchy, and their IP addresses are hard-coded into the resolver's configuration, typically provided by the operating system vendor.
   - **Root Zone Response:** 
     - The root server doesn't know the IP address of `www.google.com` but provides a referral to the `.com` Top-Level Domain (TLD) name servers, which is one step closer to the final answer.

4. **Query to the `.com` TLD Name Servers:**
   - The resolver now queries one of the `.com` TLD name servers provided by the root server.
   - **TLD Name Server Response:** 
     - The `.com` TLD server will not provide the IP address of `www.google.com` directly, but it will return the name servers responsible for `google.com`. These name servers contain authoritative information about `google.com`.

5. **Query to the `google.com` Name Servers:**
   - With the information from the `.com` TLD server, the resolver now queries the `google.com` name servers.
   - **Google Name Server Response:** 
     - The `google.com` name server responds with the IP address for `www.google.com`.

6. **Final Response and Caching:**
   - The DNS resolver finally returns the IP address to the client.
   - **Caching:** 
     - The resolver caches this result for future queries to `google.com`, speeding up subsequent resolution requests.
