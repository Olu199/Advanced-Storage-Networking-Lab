**Different Approaches to Encryption**

**Key Concepts:**
- **Symmetric Encryption**: Involves a single key used for both encryption and decryption.
- **Asymmetric Encryption**: Utilizes a pair of keys—one for encryption (public key) and another for decryption (private key).
- **Digital Signing**: Ensures data integrity and authenticity by using cryptographic signatures.
- **Steganography**: Conceals data within other non-suspicious data to prevent detection.

**Two Main Approaches to Encryption:**

1. **Encryption at Rest**:
   - **Example**: Consider a physical laptop where the storage is encrypted. The data is decrypted only when accessed by a user with the correct credentials. If the laptop is stolen, the data remains encrypted and secure.
   - **Use Case**: Typically used when only one party is involved, ensuring data is protected when not actively in use.

2. **Encryption in Transit**:
   - **Example**: When data is sent over a network, encryption in transit is applied, often through a secure tunnel (e.g., TLS/SSL). This is crucial when multiple users are involved, ensuring data remains protected as it moves between systems.

**Additional Concepts:**

- **Plaintext**: Unencrypted data in any format. It’s referred to as "plaintext" not because it's always text but because it is unprotected and can be easily read or used.
  
- **Algorithm**: A set of instructions or mathematical operations that takes plaintext and a key to produce encrypted data (ciphertext). Examples include Blowfish, AES, RC4, DES, RC5, and RC6.

- **Key**: Essentially a password used in the encryption process. In symmetric encryption, the same key is used for both encrypting and decrypting data. In asymmetric encryption, a pair of keys is used.

- **Ciphertext**: The result of encryption, where plaintext data is transformed into an unreadable format. Like plaintext, ciphertext is not always literal text but represents data in its encrypted form.

- **Decryption**: The process of converting ciphertext back into plaintext using the appropriate key.

---

### Symmetric Encryption:

- **Step 1**: Agree on the algorithm to use (e.g., AES-256).
- **Process**: The plaintext and a symmetric key are input into the symmetric encryption algorithm, producing ciphertext. To decrypt, the ciphertext is input back into the algorithm using the same symmetric key.
- **Limitation**: Symmetric encryption becomes vulnerable during digital transmission. The key must be transferred securely, but in transit, it is susceptible to interception by bad actors.

### Asymmetric Encryption:

- **Step 1**: Agree on the algorithm to use.
- **Structure**: Unlike symmetric encryption, asymmetric encryption uses two keys: a public key and a private key.
- **Process**: 
  - Each party generates a public and private key pair.
  - The public key is used to encrypt data, producing ciphertext. However, the public key cannot decrypt the data—only the corresponding private key, which is kept secret, can do that.
  - Because the public key is openly shared and only the private key can decrypt the data, asymmetric encryption is considered more secure.
- **Use Case**: Asymmetric encryption is often used to securely agree on a symmetric key. Once the symmetric key is exchanged, it is used for ongoing communication due to its efficiency.


### **Encryption Does Not Prove Identity:**

- **Digital Signing**: Encryption alone doesn’t verify the identity of the sender. However, by using digital signatures, identity can be verified. In this process, the sender uses their private key to sign a message. This signed message is then sent along with the sender’s public key. The receiver can use the sender’s public key to verify that the message was indeed signed by the sender's private key, thus confirming the sender's identity.

### **Steganography:**

- **Steganography**: This technique involves concealing data within another medium so that its presence is not detected. A classic example of physical steganography is the use of invisible ink to hide a message within an otherwise ordinary document.
