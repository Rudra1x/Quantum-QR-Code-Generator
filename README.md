# ⚛️ Quantum-Secure QR Code Generator

A command-line project for generating and decoding **Post-Quantum Secure (PQC)** QR codes.

This system demonstrates how to combine **true quantum randomness (QRNG)** with **quantum-resistant cryptography (PQC)** to create a "hybrid-encryption" system for ultra-secure, offline data transfer.

![Generated QR Code]<img width="1450" height="1450" alt="image" src="https://github.com/user-attachments/assets/0ffda869-415a-4472-90e6-caf6001d3a9c" />

---

### ## ⚠️ The Problem: "Digital Postcards"

A standard QR code is a "digital postcard"—anyone who scans it can read the data (a URL, plain text, etc.). This makes them useless for sending sensitive information like:
* Passwords or API keys
* Private cryptocurrency keys
* Secure coordinates or meeting locations

Even if you encrypt it with today's standards (like RSA), the data is vulnerable to a "harvest now, decrypt later" attack by a future quantum computer.

### ## 🔐 The Solution: A PQC Hybrid-Encryption "Lockbox"

This project doesn't change the QR code itself. It fundamentally changes *what is put inside*. We wrap our secret data in multiple layers of security, creating a quantum-proof "digital lockbox."

Here is the high-level process:

1.  **⚛️ The AES Key (QRNG):** First, we use **Qiskit** to simulate a quantum circuit. This generates a **truly random 256-bit symmetric key** (`aes_key`). This is our "inner key."
2.  **🔒 The Data (AES):** We use this `aes_key` to encrypt our secret message (e.g., `"Meet at 123 Main St."`) using fast, standard **AES-256 GCM**. This creates our `encrypted_data`.
3.  **📦 The "Lockbox" (PQC):** We now have to securely give the `aes_key` to the receiver. We use a **Post-Quantum Cryptography** algorithm (**CRYSTALS-Kyber**) to encrypt the `aes_key`. This creates a quantum-resistant "lockbox" (`ciphertext_kem`) that can *only* be opened by the receiver's `receiver_secret_key`.
4.  **📨 The Final Payload:** We bundle all these pieces (`ciphertext_kem`, `encrypted_data`, etc.) into a JSON payload, convert it to Base64, and encode it into the final QR code.

The result is a QR code that is meaningless to anyone who scans it, but can be instantly and securely decrypted by the one person who holds the correct PQC secret key.

---

### ## ✨ Key Features

* **Quantum-Resistant:** Uses **CRYSTALS-Kyber**, a PQC algorithm standardized by NIST, to protect the data from future quantum computer attacks.
* **True Randomness (Simulated):** Uses **Qiskit** to generate the core encryption key from the principles of quantum superposition, making it provably unpredictable.
* **Hybrid Encryption:** Gets the best of both worlds: the high speed of symmetric AES encryption for the data and the high security of asymmetric PQC for the key.
* **Offline Data Transfer:** Perfect for "air-gapped" systems. You can securely transfer a password from an online machine to an offline server using only a camera.

---

### ## 💻 Technology Stack

* **Qiskit (`qiskit-aer`):** For simulating a Quantum Random Number Generator (QRNG).
* **pypqc:** For the CRYSTALS-Kyber PQC (Key Encapsulation Mechanism).
* **cryptography:** For the AES-256 GCM symmetric encryption.
* **qrcode (`pillow`):** For generating the final QR code image.
* **pyzbar:** For reading the QR code data from an image.

---

### ## 🚀 Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git)
    cd YOUR_REPOSITORY
    ```

2.  **Create and activate a virtual environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: .\venv\Scripts\activate
    ```

3.  **Install the requirements:**
    Create a file named `requirements.txt` with the content below and run `pip install -r requirements.txt`.

    **`requirements.txt`**
    ```
    qiskit
    qiskit-aer
    pypqc
    cryptography
    qrcode
    pillow
    pyzbar
    ```
    *Note: `pyzbar` may require additional system libraries. See its documentation if you have trouble.*

---

### ## ⚙️ How to Use

This project runs in three simple steps. Save the code blocks as separate Python files.
