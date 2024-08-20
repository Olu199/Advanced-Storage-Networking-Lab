When a digital signature is combined with hashing, it verifies both the integrity and authenticity of the data. This process provides essential information about *who* signed the data and *what* was signed, ensuring that the data hasn't been altered and confirming the identity of the entity that signed it.

### How it Works:
1. **Integrity (What):**
   - **Hashing**: A hash of the data is taken, leaving the original data unaltered. This hash serves as a unique fingerprint of the data.
   - The hash ensures that any modification to the data will result in a different hash, thereby confirming the integrity of the data.

2. **Authenticity (Who):**
   - **Digital Signing**: The hash is digitally signed using the sender’s private key, creating a digital signature.
   - The public key, which is widely distributed and trusted, can be used to verify this signature.
   - Since only the holder of the private key can generate a valid signature, this confirms the identity of the signer.

3. **Verification Process:**
   - The recipient hashes the received data using the same hash function.
   - The recipient then uses the sender’s public key to decrypt the digital signature, revealing the original hash.
   - If the newly computed hash matches the original hash from the digital signature, it verifies that the document has not been altered (integrity) and that it was signed by the holder of the private key (authenticity).

### Key Points:
- The **public key** can be widely distributed and trusted, as it does not compromise the security of the private key.
- The **hash** cannot be changed as nobody else has the private key to create a valid signature.
- If the data is altered, the signature will no longer match the hash, invalidating the digital signature.
- Trust in the public key leads to trust in the private key, which extends to trust in the entity that signed the data, and consequently, trust in the data itself.
