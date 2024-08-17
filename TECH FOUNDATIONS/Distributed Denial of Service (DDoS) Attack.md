- **Purpose**: A DDoS attack is designed to overwhelm a website or online service by flooding it with traffic, rendering it inaccessible to legitimate users.
- **Nature of Attack**: 
  - Competes against legitimate connections, making it difficult for genuine users to access the targeted service.
  - **Distributed Attack**: The attack is executed using multiple systems, often from different locations, making it difficult to block individual IP addresses or ranges.

### Types of DDoS Attacks:
1. **Application Layer Attack (HTTP Flood)**:
   - Targets the application layer by sending a large number of HTTP requests to overwhelm the server, making it unable to process legitimate requests.
   
2. **Protocol Attack (SYN Flood)**:
   - Exploits weaknesses in the TCP/IP protocol. The attacker sends a flood of SYN packets to the target server, consuming server resources and preventing legitimate connections from being established.

3. **Volumetric Attack (DNS Amplification)**:
   - Involves overwhelming the target with a large volume of traffic by exploiting vulnerable DNS servers. The attacker sends small requests to the DNS server, which then responds with much larger responses directed at the target, amplifying the traffic and exhausting the target's bandwidth.
