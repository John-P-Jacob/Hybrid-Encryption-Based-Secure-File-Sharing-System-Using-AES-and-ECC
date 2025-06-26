# Hybrid Crypto-Ransomware Simulation Lab

## Objective

This project simulates a modern **hybrid crypto-ransomware** attack to understand advanced cryptographic workflows used in real-world cyber threats. The primary focus was to design and implement a secure cryptographic system incorporating both **symmetric (AES)** and **asymmetric (ECC)** encryption to demonstrate file-level data confidentiality, integrity, and access control mechanisms. This educational simulation replicates how sophisticated ransomware encrypts and protects files, manages secure key exchange, and implements layered encryption schemes without involving malicious code execution.

The project provides hands-on experience with cryptographic concepts essential for cybersecurity professionals, including secure key management, digital envelopes, signature verification, and the mathematical foundations underlying modern ransomware attacks and defensive countermeasures.

### Skills Learned

- **Advanced Cryptographic Implementation**: Hands-on experience with hybrid cryptographic schemes combining symmetric and asymmetric encryption
- **Elliptic Curve Cryptography (ECC)**: Deep understanding of ECC key generation, encryption, and digital signature implementation
- **Secure Key Management**: Implementation of ephemeral session keys, key wrapping, and secure key derivation functions
- **Digital Envelope Architecture**: Understanding of how public key infrastructure protects sensitive cryptographic material
- **OpenSSL Proficiency**: Advanced usage of OpenSSL CLI for cryptographic operations and certificate management
- **Cryptographic Protocol Design**: Development of secure multi-step encryption and decryption workflows
- **Bash Scripting for Security**: Automated cryptographic workflows with secure temporary file handling
- **Threat Modeling**: Analysis of real-world ransomware architecture and attack vectors
- **Cryptographic Hygiene**: Implementation of secure coding practices for cryptographic applications
- **Forward Secrecy Concepts**: Understanding of ephemeral key generation and perfect forward secrecy

### Tools Used

- **OpenSSL CLI**: ECC key pair generation, AES encryption/decryption, digital signature creation and verification
- **Bash Scripting**: Automated sender and receiver workflow implementation with secure file handling
- **SHA-512 Hashing Algorithm**: File integrity verification and cryptographic hash validation
- **PBKDF2 Key Derivation Function**: Secure key derivation with salt-based hardening against brute-force attacks
- **ZIP Compression Utility**: Secure bundling of encrypted content with metadata preservation
- **ECC Curves**: Implementation using prime256v1 and other NIST-recommended elliptic curves
- **AES-256-GCM**: Advanced Encryption Standard in Galois/Counter Mode for authenticated encryption
- **File System Tools**: Secure temporary file creation and automatic cleanup mechanisms

## Steps

### Step 1: Cryptographic Architecture Design and Planning
Designed a comprehensive hybrid encryption system that mirrors real-world ransomware cryptographic workflows while maintaining educational focus and security best practices.

**System Architecture Overview:**
The simulation implements a sophisticated multi-layer encryption scheme where each file is encrypted using a unique AES session key, which is then protected using ECC public key cryptography. The sender's private key is secured using a digital envelope technique, requiring the attacker's private key for access.

**Security Design Principles:**
- **Forward Secrecy**: Each file uses a unique ephemeral AES session key
- **Non-repudiation**: Digital signatures ensure authenticity and detect tampering
- **Access Control**: Multi-layer key protection prevents unauthorized decryption
- **Secure Communication**: All sensitive keys are cryptographically protected during transmission

### Step 2: ECC Key Infrastructure Setup
Established a comprehensive Elliptic Curve Cryptography key infrastructure supporting multiple participants (receivers and sender) with proper key management practices.

**Key Generation Process:**
Generated multiple ECC key pairs using OpenSSL with the prime256v1 curve, creating a realistic multi-party cryptographic environment:
- **Three Receiver Key Pairs**: receiver1.pub/priv, receiver2.pub/priv, receiver3.pub/priv
- **Sender Key Pair**: sender.pub/sender.priv for digital signature and key recovery operations
- **Secure Key Storage**: All private keys generated with appropriate file permissions and secure storage practices

**Cryptographic Parameters:**
- **Elliptic Curve**: prime256v1 (NIST P-256) for optimal security-performance balance
- **Key Size**: 256-bit keys providing equivalent security to 3072-bit RSA
- **Hashing Algorithm**: SHA-512 for all signature and integrity verification operations

### Step 3: Hybrid Encryption Implementation - Sender Workflow
Developed the core encryption workflow that demonstrates how ransomware encrypts victim files using a sophisticated multi-stage process.

**Sender Workflow Implementation:**
```bash
./crypto.sh -sender <receiver1_pub> <receiver2_pub> <receiver3_pub> <sender_priv> <plaintext_file> <zip_filename>
```

**Multi-Stage Encryption Process:**

**Stage 1 - Session Key Generation:**
- Generated cryptographically secure random AES-256 session key for each target file
- Applied PBKDF2 key derivation function with random salt for additional security hardening
- Ensured each file uses a unique encryption key for forward secrecy

**Stage 2 - File Encryption:**
- Encrypted target file using AES-256-GCM (Galois/Counter Mode) for authenticated encryption
- GCM mode provides both confidentiality and integrity protection in a single operation
- Generated authentication tags to detect any tampering attempts during transmission

**Stage 3 - Session Key Protection:**
- Encrypted the AES session key using the receiver's ECC public key
- Implemented proper ECIES (Elliptic Curve Integrated Encryption Scheme) for hybrid encryption
- Ensured only the holder of the corresponding private key can recover the session key

### Step 4: Digital Signature Implementation for Authentication
Implemented comprehensive digital signature mechanisms to ensure file authenticity and detect tampering attempts.

**Digital Signature Workflow:**
- Generated ECDSA (Elliptic Curve Digital Signature Algorithm) signatures over encrypted file content
- Used sender's private key to create non-repudiable proof of origin
- Implemented proper signature verification in the receiver workflow
- Applied SHA-512 hashing before signature generation for optimal security

**Anti-Tampering Measures:**
The signature verification process ensures that any modification to the encrypted file, metadata, or transmission will result in verification failure, preventing successful decryption and alerting to potential attacks.

### Step 5: Digital Envelope Architecture for Key Protection
Implemented advanced digital envelope techniques to protect the sender's private key, simulating how ransomware operators protect their decryption capabilities.

**Digital Envelope Implementation:**
- Encrypted sender's private key using the attacker's (receiver's) public key
- Created secure key escrow mechanism preventing unauthorized access to decryption capabilities
- Implemented proper key wrapping protocols following cryptographic best practices
- Ensured forward secrecy by protecting all decryption keys behind additional encryption layers

This architecture mirrors real-world ransomware where victim keys are protected by the attacker's master keys, requiring payment and cooperation for key release.

### Step 6: Secure Package Creation and Metadata Management
Developed secure packaging mechanisms to bundle all cryptographic components while maintaining security and integrity.

**Package Components:**
- **Encrypted File Data**: AES-256-GCM encrypted original file content
- **Encrypted Session Key**: ECC-protected AES key required for file decryption  
- **Digital Signature**: ECDSA signature for authenticity verification
- **Protected Private Key**: Digital envelope containing sender's private key
- **Metadata**: Cryptographic parameters, salt values, and algorithm specifications

**Secure Bundling Process:**
Created ZIP archives containing all necessary components for decryption while implementing secure temporary file handling to prevent key leakage during processing.

### Step 7: Receiver Workflow Implementation and Key Recovery
Developed the complete decryption workflow demonstrating how authorized parties can recover encrypted data through proper cryptographic key management.

**Receiver Workflow Implementation:**
```bash
./crypto.sh -receiver <receiver_priv> <sender_pub> <zip_file> <output_plaintext_file>
```

**Multi-Stage Decryption Process:**

**Stage 1 - Digital Envelope Opening:**
- Used receiver's private key to decrypt and recover sender's private key
- Implemented secure key recovery with proper error handling for invalid keys
- Validated key format and cryptographic parameters before proceeding

**Stage 2 - Signature Verification:**
- Verified ECDSA signature using sender's public key to ensure file authenticity
- Implemented fail-secure mechanism that prevents decryption if signature verification fails
- Protected against tampering and man-in-the-middle attacks through cryptographic validation

**Stage 3 - Session Key Recovery:**
- Decrypted AES session key using recovered sender private key
- Applied proper key derivation functions to reconstruct original encryption key
- Validated key parameters and cryptographic consistency

**Stage 4 - File Decryption:**
- Decrypted original file using recovered AES-256-GCM session key
- Verified authentication tags to ensure data integrity during decryption
- Applied secure file writing practices to prevent data corruption

### Step 8: Cryptographic Validation and Integrity Verification
Implemented comprehensive validation mechanisms to ensure successful encryption and decryption operations while maintaining cryptographic security.

**Hash-Based Integrity Verification:**
- Generated SHA-512 hashes of original files before encryption
- Computed SHA-512 hashes of decrypted files after successful decryption
- Performed bitwise comparison to validate perfect data recovery
- Implemented automated hash matching to detect any data corruption or incomplete decryption

**Security Validation Checks:**
- **Signature Verification**: Mandatory ECDSA signature validation before decryption
- **Key Validation**: Verification of ECC key parameters and format compliance
- **Decryption Validation**: Authentication tag verification during AES-GCM decryption
- **File Integrity**: Hash-based validation of complete data recovery

### Step 9: Secure File Handling and Cleanup Implementation
Developed secure temporary file management and cleanup procedures to prevent cryptographic key leakage and maintain operational security.

**Secure Temporary File Management:**
- Created temporary files with restrictive permissions (600) to prevent unauthorized access
- Implemented secure random filename generation to prevent predictable temporary file attacks
- Applied proper file locking mechanisms during cryptographic operations

**Automatic Cleanup Procedures:**
- **Post-Encryption Cleanup**: Automatic deletion of all intermediate files after ZIP creation
- **Post-Decryption Cleanup**: Secure deletion of temporary decryption files after successful completion
- **Error-State Cleanup**: Cleanup procedures activated even during error conditions to prevent key leakage
- **Memory Security**: Avoided storing sensitive keys in environment variables or command history

### Step 10: Fail-Secure Implementation and Error Handling
Implemented comprehensive error handling and fail-secure mechanisms to prevent cryptographic bypasses and ensure system security under all conditions.

**Fail-Secure Conditions:**
The system implements secure failure modes for several critical security conditions:

**Signature Verification Failure:**
- System immediately terminates decryption process if ECDSA signature verification fails
- Prevents tampering attacks and ensures file authenticity
- Logs security events for forensic analysis

**Missing or Invalid Keys:**
- Validates all cryptographic keys before processing
- Terminates operations with secure cleanup if keys are missing, corrupted, or invalid
- Prevents partial decryption that could leak information about plaintext

**Incorrect Decryption Sequence:**
- Enforces proper cryptographic workflow order
- Prevents cryptographic oracle attacks through improper operation sequencing
- Maintains security even if attacker attempts to manipulate the decryption process

### Step 11: Advanced Cryptographic Analysis and Testing
Conducted comprehensive testing and validation of the cryptographic implementation to ensure security properties and identify potential vulnerabilities.

**Cryptographic Property Validation:**

**Forward Secrecy Testing:**
- Verified that compromise of long-term keys does not compromise past session keys
- Tested ephemeral key generation and secure deletion procedures
- Validated that each file encryption uses unique, non-reusable session keys

**Key Derivation Security:**
- Tested PBKDF2 implementation with various iteration counts and salt values
- Validated resistance to rainbow table and dictionary attacks
- Verified proper salt generation and storage mechanisms

**Signature Security Analysis:**
- Tested ECDSA implementation against known attack vectors
- Validated signature verification under various tampering scenarios
- Ensured non-repudiation properties of digital signatures

**Performance and Security Balance:**
- Analyzed encryption/decryption performance with different file sizes
- Optimized cryptographic operations while maintaining security properties
- Tested system behavior under resource constraints and error conditions

## Key Achievements and Lessons Learned

### Cryptographic Implementation Mastery
This project provided comprehensive hands-on experience with advanced cryptographic concepts essential for cybersecurity professionals:

- **Hybrid Cryptography**: Gained practical understanding of combining symmetric and asymmetric encryption for optimal security and performance
- **ECC Implementation**: Developed expertise in elliptic curve cryptography including key generation, encryption, and digital signatures
- **Secure Protocol Design**: Learned to design and implement secure multi-step cryptographic protocols with proper error handling

### Real-World Security Applications
The skills developed directly apply to professional cybersecurity scenarios:

- **Incident Response**: Understanding of ransomware cryptographic mechanisms aids in attack analysis and recovery planning
- **Malware Analysis**: Knowledge of encryption schemes helps in reverse engineering and threat assessment
- **Cryptographic Security**: Ability to implement and validate secure cryptographic systems for enterprise applications
- **Threat Modeling**: Understanding of attacker cryptographic capabilities and defensive countermeasures

### Advanced Technical Skills
- **OpenSSL Proficiency**: Advanced command-line cryptographic operations and certificate management
- **Secure Scripting**: Development of security-focused automation with proper error handling and cleanup
- **Cryptographic Hygiene**: Implementation of secure coding practices for cryptographic applications

## Future Enhancements and Extensions

### Advanced Cryptographic Features
Potential expansions to further develop the cryptographic simulation:

- **Network-Based Key Exchange**: Integration of C2 communication protocols for realistic key distribution
- **Multi-Party Key Splitting**: Implementation of Shamir's Secret Sharing for distributed key recovery
- **Time-Lock Encryption**: Addition of temporal access controls and key escrow mechanisms
- **Post-Quantum Cryptography**: Integration of quantum-resistant algorithms for future-proof security

### Forensic and Analysis Extensions
- **Memory Forensics**: Analysis of RAM dumps for cryptographic key leakage detection
- **Side-Channel Analysis**: Testing for timing and power analysis vulnerabilities
- **Cryptographic Oracle Testing**: Advanced attack simulation against cryptographic implementations

## Conclusion

This Hybrid Crypto-Ransomware Simulation project successfully demonstrated the ability to implement, analyze, and understand advanced cryptographic systems used in modern cyber threats. The hands-on experience with ECC, AES, digital signatures, and secure key management provides a solid foundation for cybersecurity roles focused on malware analysis, incident response, and cryptographic security.

The project's emphasis on secure implementation practices, comprehensive error handling, and cryptographic validation mirrors the rigorous security requirements of professional cybersecurity environments. The combination of theoretical cryptographic knowledge with practical implementation skills demonstrates readiness for advanced technical roles in cybersecurity and digital forensics.
