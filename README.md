# keylogger-project
Proof-of-Concept: Encrypted Keylogger with Data

Exfiltration

!! For Educational and Ethical Use Only !!
Based on user-provided project outline

October 26, 2025

Abstract

This report details the construction of a proof-of-concept (PoC) keylogger designed for edu-
cational purposes. The tool captures keystrokes, encrypts them using symmetric-key cryp-
tography (Fernet), stores the logs locally, and simulates data exfiltration to a local server.

This document provides the full source code and concludes with a critical discussion on the
ethical, legal, and moral implications of such software.

1 Objective
The primary objective of this project is to build a functional, proof-of-concept keylogger that
demonstrates a basic cyber-attack chain. This includes:
• Capturing user keystrokes.
• Encrypting the captured data to obscure its content.
• Storing the encrypted logs locally for persistence.
• Simulating data exfiltration by sending the encrypted logs to a remote server.
This PoC serves as a practical learning tool for understanding offensive security techniques and,
more importantly, the defensive measures required to counter them (e.g., endpoint detection,
network monitoring, and encryption key management).
2 Tools and Methodology
2.1 Tools Used
The PoC is built using Python 3 and relies on the following key libraries:
• Python 3: The core programming language.
• pynput: A library for capturing and controlling user input devices, used here to listen for
keyboard events.
• cryptography: A robust library providing cryptographic recipes. We specifically use cryptography.fernet
for high-level symmetric (Fernet) encryption.
• base64: Used to safely encode the binary encrypted data into ASCII text for transport over
HTTP.
• requests: A simple HTTP library for sending the exfiltrated data.
• http.server: A built-in Python module used to create the simple receiver server.
2.2 Methodology
The project is split into three separate Python scripts for clarity and modularity:
1. Key Generation (generate_key.py): A utility script that generates a symmetric Fernet key
and saves it to a file (key.key). This key is required by both the keylogger (for encryption)
and the server (for decryption).
2. Keylogger (keylogger.py): This is the main client-side script. It loads the encryption key,
starts a pynput listener to capture keystrokes, and stores them in a buffer. Periodically, it
encrypts this buffer, saves it to a local file (keylog.enc), and sends the encrypted data via
an HTTP POST request to the receiver server.
3. Receiver Server (receiver_server.py): This simple server listens on localhost:8000. When

it receives a POST request, it attempts to load the same key.key, decrypt the received pay-
load, and print the original keystrokes to the console.

Note: The ”startup persistence and kill switch” (step ’e’ from the prompt) have been omitted. Per-
sistence mechanisms are highly platform-specific and move significantly beyond a simple PoC int
malicious tool-craft. A simple ’ESC’ key listener is commented in the code as a potential kill switch.
