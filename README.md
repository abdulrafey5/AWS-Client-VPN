# AWS Client VPN Project

## Overview
This document provides the commands necessary to create server and client certificates and keys using EasyRSA, as well as instructions for setting up the directory structure and copying the necessary PKI files.

---

## Prerequisites
- **EasyRSA**: [Download and install EasyRSA](https://github.com/OpenVPN/easy-rsa/releases/tag/v3.1.7).
- **Command Line Access**: Use PowerShell or Command Prompt on Windows, and a terminal on Linux/Mac.
  
---

## Step 1: Create Server and Client Certificates with EasyRSA

1. **Start EasyRSA**:
   ```powershell
   .\EasyRSA-Start.bat
   ```

2. **Initialize PKI**:
   Initialize the Public Key Infrastructure (PKI) directory.
   ```bash
   ./easyrsa init-pki
   ```

3. **Build Certificate Authority (CA)**:
   Create a new certificate authority without a passphrase.
   ```bash
   ./easyrsa build-ca nopass
   ```

4. **Create Server Certificate**:
   Generate the server certificate without a passphrase.
   ```bash
   ./easyrsa build-server-full server nopass
   ```

5. **Create Client Certificate**:
   Generate a client certificate for the specified domain without a passphrase.
   ```bash
   ./easyrsa build-client-full client1.domain.tld nopass
   ```

6. **Exit EasyRSA**:
   ```bash
   exit
   ```

---

## Step 2: Create Directory and Copy PKI Content

After generating the certificates, create a directory to store them and copy the necessary PKI files.

1. **Create Directory**:
   Create a directory called `vpncert` to store the certificates and keys.
   ```powershell
   mkdir C:\vpncert
   ```

2. **Copy PKI Files**:
   Copy the CA certificate, server certificate, server key, client certificate, and client key to the `vpncert` directory.

   ```powershell
   copy pki\ca.crt C:\vpncert
   copy pki\issued\server.crt C:\vpncert
   copy pki\private\server.key C:\vpncert
   copy pki\issued\client1.domain.tld.crt C:\vpncert
   copy pki\private\client1.domain.tld.key C:\vpncert
   ```

3. **Navigate to the Directory**:
   Move to the `vpncert` directory to verify that all necessary files are present.
   ```powershell
   cd C:\vpncert
   ```

---

## Summary
You have now generated the necessary server and client certificates for AWS Client VPN and organized them in a designated directory.

## Next Steps
- Use these certificates in AWS Certificate Manager (ACM) to configure your AWS Client VPN endpoint.
