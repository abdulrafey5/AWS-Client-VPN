# AWS Client VPN Project

## Overview
This document provides the commands necessary to create server and client certificates and keys using EasyRSA, as well as instructions for setting up the directory structure and copying the necessary PKI files.

---

## Prerequisites
- **EasyRSA**: [Download and install EasyRSA](https://github.com/OpenVPN/easy-rsa/releases/tag/v3.1.7).
- **Command Line Access**: Use PowerShell or Command Prompt on Windows, and a terminal on Linux/Mac.
- **VPC,subnets and EC2**: Create a VPC, two subnets(clientvpn-network-interface and target server), an EC2 instance(enable RDP and ICMP access in case of Windows). 
  
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

## Step 3: Upload Certificates to AWS Certificate Manager (ACM)
1. Go to the AWS Certificate Manager console.
2. Choose **Import a certificate** and upload:
   - `ca.crt` (Certificate Authority file)
   - `server.crt` and `server.key` (Server certificate and key)

## Step 4: Set Up AWS Client VPN Endpoint
1. Navigate to **VPC > Client VPN Endpoints** in AWS.
2. Create a new Client VPN endpoint, selecting the uploaded server certificate.
3. Associate the endpoint with your desired VPC subnets and configure the route table.


## Testing and Validation
- Test the VPN connection by connecting with a VPN client.
- Troubleshoot connection issues by verifying your AWS VPC and route configurations.

## Security Considerations
- Keep private keys secure and avoid sharing sensitive information publicly.
