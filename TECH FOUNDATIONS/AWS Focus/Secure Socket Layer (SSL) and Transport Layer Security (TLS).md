- **SSL** uses asymmetric encryption.
- **TLS** uses symmetric encryption.
- Both SSL and TLS provide privacy and data integrity between client and server. Although they perform similar functions, TLS is the newer and more secure version.

### Functions of TLS:
1. **Privacy**: Encrypts communication between client and server.
2. **Identity Verification**: Ensures both parties are who they claim to be. TLS supports two-way communication.

### SSL/TLS Process (Begins at Layer 4):
1. **Cipher Suites**: 
   - **Client Hello**: The client sends a "hello" message containing the SSL/TLS version, a list of supported cipher suites, session ID, and extensions. This is the first step in the handshake over the TCP protocol. The server, in this bi-directional communication, has a certificate.
   - **Server Hello**: The server responds with its own "hello," which includes a certificate containing its public key. If SSL is used, asymmetric encryption introduces additional computational load on the server.

2. **Authentication**:
   - Ensures the server's certificate is authentic. A public trusted certificate authority (CA) is trusted by the client’s OS. 
   - Before authentication, the server generates a public and private key pair and a certificate signing request. The CA signs the request during the handshake process. If the client’s OS or browser trusts the certificate, it verifies the signed certificate by checking the certificate's date, revocation status, names, and DNS name.
   - The client then confirms that the server possesses the private key, verifying the server's identity.

3. **Key Exchange**:
   - After verifying identity, the process moves from asymmetric to symmetric encryption. The client generates a pre-master key, encrypts it with the server's public key, and sends it to the server.
   - The server decrypts the pre-master key using its private key. Both sides then generate the master secret, allowing the encryption process to begin securely.