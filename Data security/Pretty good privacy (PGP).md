<span style="color:rgb(216, 203, 251)">Widely utilized and ecryption system</span> based on the principals of public-key cryptography.
Practical applications:
- Email security.
- Signed git commits.
- File encryption.
- Verifiable downloads.
- Passwordless authentication.
# Encryption
Only an intended recipient can <span style="color:rgb(216, 203, 251)">read a document</span>.

| Key type    | Distribution                                                                                    | Function                                                       |
| ----------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Public key  | <span style="color:rgb(216, 203, 251)">Distributed as widely as possible</span> on the internet | Encrypt messages for the owner or verify the owner's signature |
| Private key | Kept as a <span style="color:rgb(216, 203, 251)">closely guarded secret by the owner</span>     | Decrypt incoming messages or to sign outgoing documents        |
## Encryption process
1. **Process:** <span style="color:rgb(216, 203, 251)">the sender uses the recipient's public key</span> to encrypt a plain text document.
2. **Result:** a <span style="color:rgb(216, 203, 251)">meaningless string of characters is produced</span>.
3. **Decryption:** the recipient, using the private key, can <span style="color:rgb(216, 203, 251)">decode the message back into plain text</span>.
# Signing
<span style="color:rgb(216, 203, 251)">Verifies the identity of the sender</span> and ensures the integrity of the data. The most significant challenge is identity verification: <span style="color:rgb(216, 203, 251)">ensuring a public key truly belongs to the individual it claims to represent</span>.
- PGP addresses this through the "Web of Trust" a descentralized model where <span style="color:rgb(216, 203, 251)">users sign each other's keys to vouch for identities</span>.
## Signing process
1. **Process:** the sender <span style="color:rgb(216, 203, 251)">runs a document through a signing program</span> using their private key.
2. **Result:** the document remains readable (plain text), but a <span style="color:rgb(216, 203, 251)">digital signature is attached</span>.
3. **Verification:** the <span style="color:rgb(216, 203, 251)">recipient uses the sender's public key to verify the signature</span>. If verified, it proves the document was signed by that specific private key.
# PGP in Data Engineering
![[Drawing 2026-05-23 11.17.50.excalidraw]]