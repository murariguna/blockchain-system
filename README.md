# 🏥 Electronic Health Record Blockchain Based Platform

A blockchain-powered Electronic Health Record (EHR) platform built using Hyperledger Fabric, Node.js, Docker, and IPFS.

This project demonstrates how enterprise blockchain can be used for:

* Secure patient record management
* Decentralized healthcare data sharing
* Role-based access control
* Immutable medical audit logs
* Multi-organization healthcare ecosystems

---

# 🚀 Tech Stack

| Technology                 | Purpose                                  |
| -------------------------- | ---------------------------------------- |
| Hyperledger Fabric         | Enterprise blockchain framework          |
| Node.js                    | Backend APIs and SDK integration         |
| Fabric Node SDK            | Blockchain interaction                   |
| Docker                     | Containerized blockchain deployment      |
| Hyperledger Explorer       | Blockchain visualization and monitoring  |
| Angular / Next.js Frontend | User interface                           |
| CouchDB                    | State database                           |
| IPFS                       | Decentralized file storage               |
| WSL2                       | Linux development environment on Windows |

---

# 🧱 Project Architecture

```text
Frontend UI
    ↓
Node.js Backend APIs
    ↓
Hyperledger Fabric SDK
    ↓
Smart Contracts (Chaincode)
    ↓
Fabric Blockchain Network
 ├── Org1 (Hospital)
 ├── Org2 (Insurance)
 ├── Orderer
 ├── Certificate Authorities
 ├── CouchDB
 └── Hyperledger Explorer
```

---

# 📂 Project Structure

```text
EHR-Hyperledger-Fabric-Project/
│
├── fabric-samples/                  # Hyperledger Fabric samples and network
├── server-node-sdk/                 # Backend APIs and Fabric SDK scripts
├── fabric-explorer/                 # Hyperledger Explorer setup
├── backup/                          # Backup utility scripts
├── screenshots/                     # Project screenshots
├── README.md
└── install-fabric.sh
```

---

# ⚙️ Prerequisites

Before starting, install:

* Docker Desktop
* WSL2 Ubuntu
* Node.js >= 20
* npm >= 10
* Git
* VS Code with WSL Extension

---

# 🔧 Steps to Setup Project

---

# 1️⃣ Download Fabric Binaries and Samples

```bash
./install-fabric.sh
```

This downloads:

* Fabric binaries
* Docker images
* Fabric samples repository

---

# 2️⃣ Start Fabric Test Network

```bash
cd fabric-samples/test-network
./network.sh up
```

Check containers:

```bash
docker ps
```

Stop network:

```bash
./network.sh down
```

---

# 3️⃣ Create Channel and Start CA

```bash
cd fabric-samples/test-network
./network.sh up createChannel -ca
```

This will:

* Start peers
* Start orderer
* Start CA servers
* Create `mychannel`

---

# 4️⃣ Deploy Chaincode

```bash
./network.sh deployCC -ccn ehrChainCode -ccp ../asset-transfer-basic/chaincode-javascript/ -ccl javascript
```

If deployment succeeds, you should see:

```text
Chaincode initialization is not required
```

---

# 5️⃣ Register Admin Identities

```bash
cd server-node-sdk/

node cert-script/registerOrg1Admin.js
node cert-script/registerOrg2Admin.js
```

---

# 6️⃣ Onboard Network Participants

```bash
node cert-script/onboardHospital01.js
node cert-script/onboardDoctor.js
node cert-script/onboardInsuranceCompany.js
node cert-script/onboardInsuranceAgent.js
```

These scripts:

* Register identities
* Enroll users
* Store credentials in wallet
* Invoke blockchain transactions

---

# 7️⃣ Start Backend Server

```bash
npm install
npm run dev
```

Backend runs on:

```text
http://localhost:5000
```

---

# 📡 API List

| API                    | Purpose                    |
| ---------------------- | -------------------------- |
| registerPatient        | Register patient           |
| loginPatient           | Login patient              |
| grantAccess            | Grant access to doctor     |
| addRecord              | Add patient medical record |
| getRecordById          | Fetch record by ID         |
| getAllRecordByPatienId | Fetch patient records      |
| fetchLedger            | Fetch blockchain ledger    |

---

# 🔗 Chaincode Logic

## Actors in Network

1. Government - Network owner
2. Hospital - Network organization
3. Doctor - Hospital member
4. Diagnostics Center
5. Pharmacies
6. Researchers / R&D
7. Insurance Companies
8. Patient

---

# 🔐 Read/Write Permissions

| Actor               | Permissions                   |
| ------------------- | ----------------------------- |
| Government          | Admin Access                  |
| Hospital            | Read/Write Doctor Data        |
| Doctor              | Read/Write Patient Data       |
| Diagnostics Center  | Read/Write Diagnostic Records |
| Pharmacies          | Read/Write Prescriptions      |
| Researchers         | Read-only based on consent    |
| Insurance Companies | Read/Write Claims             |
| Patient             | Full access to own data       |

---

# 🗃️ Ledger Object Structure

## Hospital Object

```json
{
  "recordType": "hospital",
  "createdBy": "hospitalId",
  "data": {
    "name": "ABC Hospital",
    "address": "ABC Location"
  }
}
```

## Physician Object

```json
{
  "recordType": "physician",
  "createdBy": "physicianID",
  "data": {
    "name": "Doctor Name",
    "hospital": "ABC Hospital"
  }
}
```

---

# 📊 Hyperledger Explorer Setup

---

## Step 0: Go to Explorer Folder

```bash
cd fabric-explorer/
```

---

## Step 1: Copy Organizations Folder

```bash
sudo cp -r ../fabric-samples/test-network/organizations/ .
cp -r ../fabric-samples/test-network/organizations/ .
```

---

## Step 2: Export Environment Variables

```bash
export EXPLORER_CONFIG_FILE_PATH=./config.json
export EXPLORER_PROFILE_DIR_PATH=./connection-profile
export FABRIC_CRYPTO_PATH=./organizations
```

---

## Step 3: Configure test-network.json

Inside `adminPrivateKey`:

* verify certificate path
* ensure key filename matches crypto materials

---

## Step 4: Start Explorer

Without logs:

```bash
docker-compose up -d
```

With logs:

```bash
docker-compose up
```

---

## Step 5: Stop Explorer

```bash
docker-compose down
```

---

# 📸 Project Features Demonstrated

✅ Multi-Organization Fabric Network

✅ Chaincode Deployment

✅ Smart Contract Invocation

✅ Node SDK Integration

✅ Blockchain Explorer Monitoring

✅ Dockerized Infrastructure

✅ CA-Based Identity Management

✅ CouchDB State Database

✅ Healthcare Role Management

---

# 🐳 Docker Containers Used

The project uses multiple containers:

* peer0.org1.example.com
* peer0.org2.example.com
* orderer.example.com
* ca_org1
* ca_org2
* ca_orderer
* couchdb0
* couchdb1
* fabric-explorer
* chaincode containers

---

# 🧠 Major Problems Faced & Solutions

One of the biggest learning experiences in this project was debugging and fixing real enterprise blockchain setup issues.

---

# ⚠️ 1. WSL Issues

## Problem

VS Code was not opening properly in WSL.

## Solution

Installed:

* WSL2
* VS Code WSL extension

Used:

```bash
code .
```

Also fixed PATH configuration issues.

---

# ⚠️ 2. Docker Socket Broken Pipe Error

## Problem

```text
write unix @->/var/run/docker.sock: write: broken pipe
```

while deploying chaincode.

## Cause

Corrupted/stale Fabric Docker images.

## Solution

Removed old images:

```bash
docker rmi hyperledger/fabric-peer:latest -f
docker rmi hyperledger/fabric-orderer:latest -f
docker rmi hyperledger/fabric-ca:latest -f
```

Then restarted Docker Desktop.

---

# ⚠️ 3. npm + WSL Path Conflict

## Problem

npm was using Windows binaries inside WSL.

Errors:

```text
UNC paths are not supported
```

## Solution

Disabled Windows PATH integration in WSL.

Verified:

```bash
which node
which npm
```

Expected:

```text
/usr/bin/node
/usr/bin/npm
```

---

# ⚠️ 4. Angular Version Mismatch

## Problem

Angular CLI and Angular core versions were incompatible.

## Solution

Installed compatible versions:

```bash
npm install --legacy-peer-deps
npm install @types/node@16 --save-dev
```

---

# ⚠️ 5. Git Submodule Problem

## Problem

`fabric-samples` was treated as embedded Git repository.

## Solution

Removed nested Git metadata:

```bash
rm -rf fabric-samples/.git
```

Then re-added project files.

---

# ⚠️ 6. Chaincode Function Not Found

## Problem

```text
You've asked to invoke a function that does not exist: onboardDoctor
```

## Cause

Backend SDK called function not implemented in deployed chaincode.

## Solution

Updated and redeployed correct chaincode.

---

# ⚠️ 7. Node.js Version Compatibility

## Problem

Old Node.js versions caused Fabric SDK failures.

## Solution

Upgraded to:

```text
Node.js v20+
```

---

# ⚠️ 8. Explorer Crypto Path Errors

## Problem

Explorer failed to load network.

## Solution

Correctly copied:

```bash
organizations/
```

and updated:

```text
adminPrivateKey path
```

inside Explorer connection profile.

---

# 💡 Key Learning Outcomes

This project provided practical experience in:

* Enterprise blockchain development
* Smart contract deployment
* Docker container orchestration
* Distributed ledger architecture
* Blockchain identity management
* Hyperledger Fabric administration
* Real-world debugging
* WSL Linux development
* Node SDK integration
* Blockchain monitoring and analytics

---

# 📈 Future Improvements

Planned enhancements:

* Full healthcare frontend UI
* JWT authentication
* IPFS encrypted medical file storage
* Role-based dashboards
* Patient consent workflows
* Blockchain analytics
* Audit reports
* REST API documentation
* Kubernetes deployment
* CI/CD pipelines

---

# 👨‍💻 Author

Murari Guna

GitHub Repository:

[https://github.com/murariguna/blockchain-system](https://github.com/murariguna/blockchain-system)

---

# 📜 License

This project uses components from Hyperledger Fabric and follows Apache 2.0 licensing where applicable.
