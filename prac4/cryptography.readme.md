# Practical-04: Cryptography

## Title
**Implementation of Encryption, Hashing and Digital Signatures Using Python**

---

## 1. Aim

To understand and implement different cryptographic techniques, including symmetric encryption, asymmetric encryption, hashing and digital signatures, using Python to protect sensitive information and maintain data security.

## 2. Introduction

Cryptography is the technique of protecting information by converting readable data into a form that cannot be easily understood by unauthorized users.

It plays an important role in maintaining the confidentiality, integrity and authenticity of digital information.

In this practical, different cryptographic techniques are implemented using Python:

- **Encryption:** Converts readable information (plaintext) into an unreadable format (ciphertext).
- **Decryption:** Converts encrypted information back into its original readable form.
- **Hashing:** Converts information into a fixed-length value to check data integrity.
- **Digital Signature:** Verifies the authenticity of a message and helps detect whether it has been modified.

## 3. Types of Cryptographic Techniques

### A. Symmetric Encryption

Symmetric encryption uses the same secret key for both encryption and decryption.

The sender encrypts the original information using a secret key, and the receiver uses the same key to decrypt it.

**Example:** AES (Advanced Encryption Standard).

It is commonly used to protect files, messages and stored information because of its efficiency.

### B. Asymmetric Encryption

Asymmetric encryption uses two different but mathematically related keys:

- **Public Key:** Can be shared with others.
- **Private Key:** Must be kept secret.

In RSA encryption, the public key is used to encrypt information, while the corresponding private key is used to decrypt it.

**Example:** RSA (Rivest–Shamir–Adleman).

### C. Hashing

Hashing converts data into a fixed-length output called a hash value or message digest.

**Example:** SHA-256 (Secure Hash Algorithm 256-bit).

Hashing is a one-way process, meaning the original information cannot normally be recovered from the hash value.

It is commonly used to check whether information has been modified.

### D. Digital Signature

A digital signature is a cryptographic technique used to verify the authenticity and integrity of digital information.

It works using two keys:

- The private key is used to create the digital signature.
- The corresponding public key is used to verify the signature.

It helps identify unauthorized modifications to signed information.

## 4. Tools and Technologies Used

**Software Requirements:**
- Python 3.x
- Google Colab / Jupyter Notebook
- GitHub

**Python Libraries:**
- `cryptography`
- `hashlib`

**Algorithms and Techniques:**
- Fernet-based symmetric encryption (AES)
- RSA asymmetric encryption
- SHA-256 hashing
- RSA-PSS digital signatures

## 5. Installation

Install the required cryptography library using the following command:

```bash
pip install cryptography
```

Note: The `hashlib` library is included with Python and does not require separate installation.

## 6. Implementation

The following Python program demonstrates encryption, decryption, hashing and digital signature verification using sample customer information.

### Python Program

```python
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes
from cryptography.exceptions import InvalidSignature
import hashlib

# Sample customer information
message = "Customer: Rahul, Booking ID: 1025"
data = message.encode()

print("=" * 55)
print("          CRYPTOGRAPHY PRACTICAL")
print("=" * 55)

# 1. SYMMETRIC ENCRYPTION USING FERNET

key = Fernet.generate_key()
cipher = Fernet(key)

encrypted_data = cipher.encrypt(data)
decrypted_data = cipher.decrypt(encrypted_data)

print("\n1. SYMMETRIC ENCRYPTION")
print("-----------------------")
print("Original Data:", message)
print("Encrypted Data:", encrypted_data)
print("Decrypted Data:", decrypted_data.decode())

# 2. HASHING USING SHA-256

hash_value = hashlib.sha256(data).hexdigest()

print("\n2. SHA-256 HASHING")
print("-----------------------")
print("Original Data:", message)
print("SHA-256 Hash:", hash_value)

# 3. RSA ASYMMETRIC ENCRYPTION

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

public_key = private_key.public_key()

rsa_message = b"Confidential customer information"

encrypted_rsa = public_key.encrypt(
    rsa_message,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)

decrypted_rsa = private_key.decrypt(
    encrypted_rsa,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)

print("\n3. RSA ASYMMETRIC ENCRYPTION")
print("-----------------------")
print("Encrypted RSA Data:", encrypted_rsa)
print("Decrypted RSA Data:", decrypted_rsa.decode())

# 4. DIGITAL SIGNATURE USING RSA

signature_private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

signature_public_key = signature_private_key.public_key()

signature = signature_private_key.sign(
    data,
    padding.PSS(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("\n4. DIGITAL SIGNATURE")
print("-----------------------")
print("Digital Signature Created Successfully.")

# 5. VERIFY DIGITAL SIGNATURE

try:
    signature_public_key.verify(
        signature,
        data,
        padding.PSS(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )

    print("Signature Verification: SUCCESS")
    print("The message is authentic and unchanged.")

except InvalidSignature:
    print("Signature Verification: FAILED")

# 6. DETECT MESSAGE TAMPERING

tampered_data = b"Modified customer information"

print("\n5. MESSAGE TAMPERING TEST")
print("-----------------------")

try:
    signature_public_key.verify(
        signature,
        tampered_data,
        padding.PSS(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )

    print("Signature Verification: SUCCESS")

except InvalidSignature:
    print("Signature Verification: FAILED")
    print("Message modification detected.")

print("\n" + "=" * 55)
print("          PRACTICAL COMPLETED")
print("=" * 55)
```

## 7. Expected Output

The program produces the following types of output:

```text
=======================================================
          CRYPTOGRAPHY PRACTICAL
=======================================================

1. SYMMETRIC ENCRYPTION
-----------------------
Original Data: Customer: Rahul, Booking ID: 1025
Encrypted Data: <generated encrypted data>
Decrypted Data: Customer: Rahul, Booking ID: 1025

2. SHA-256 HASHING
-----------------------
Original Data: Customer: Rahul, Booking ID: 1025
SHA-256 Hash: <64-character hexadecimal hash>

3. RSA ASYMMETRIC ENCRYPTION
-----------------------
Encrypted RSA Data: <encrypted binary data>
Decrypted RSA Data: Confidential customer information

4. DIGITAL SIGNATURE
-----------------------
Digital Signature Created Successfully.
Signature Verification: SUCCESS
The message is authentic and unchanged.

5. MESSAGE TAMPERING TEST
-----------------------
Signature Verification: FAILED
Message modification detected.

=======================================================
          PRACTICAL COMPLETED
=======================================================
```

**Note:** The encryption output, RSA keys and digital signature are generated during execution and may differ between runs. The SHA-256 hash remains the same for exactly the same input.

## 8. Observations

1. Symmetric encryption successfully converted the original information into encrypted data.
2. Decryption recovered the original information using the same secret key.
3. SHA-256 generated a fixed-length hash value of 256 bits for the given information.
4. RSA successfully encrypted information using the public key and decrypted it using the corresponding private key.
5. A digital signature was successfully created using the private key.
6. The public key successfully verified the original digital signature.
7. When the original message was modified, signature verification failed, demonstrating the detection of data tampering.

## 9. Applications of Cryptography

1. **Online Banking:** Cryptography helps protect financial information and banking transactions.
2. **Digital Payments:** Cryptographic techniques help secure payment information and authenticate transactions.
3. **Secure Messaging:** Encryption helps protect private conversations from unauthorized access.
4. **Password Protection:** Hashing techniques help securely store password representations.
5. **E-commerce:** Cryptography protects customer information and online transactions.
6. **Digital Documents:** Digital signatures help verify the authenticity and integrity of electronic documents.
7. **HTTPS:** Cryptographic protocols help establish secure communication between web browsers and servers.

## 10. Advantages of Cryptography

1. Protects sensitive information from unauthorized access.
2. Maintains data confidentiality and integrity.
3. Helps verify the authenticity of information.
4. Enables secure communication over networks.
5. Helps protect online financial transactions.
6. Detects unauthorized modifications through digital signatures.

## 11. Limitations of Cryptography

1. Poor key management can compromise data security.
2. Strong cryptographic operations may require additional computational resources.
3. Losing private keys can make encrypted information inaccessible.
4. Weak passwords and poorly protected keys can reduce security.
5. Incorrect implementation can create security vulnerabilities.

## 12. Result

The practical was successfully performed using Python to demonstrate different cryptographic techniques.

The implementation covered:

- Symmetric encryption and decryption using Fernet.
- Asymmetric encryption and decryption using RSA.
- SHA-256 hashing for generating message digests.
- Digital signature generation using RSA.
- Digital signature verification using the public key.
- Detection of message tampering.

Through this practical, I understood how encryption protects confidential information, hashing helps maintain data integrity, and digital signatures verify authenticity.

Thus, cryptography plays an important role in maintaining data privacy and information security.
