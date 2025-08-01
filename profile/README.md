# Oracle Mining Process

## Overview

This document outlines the end-to-end oracle mining process in the protocol, detailing how off-chain property data is validated, transformed, and anchored on-chain. The oracle mining process is the core mechanism by which decentralized property records are created and maintained.

### Oracle Mining Process

The protocol relies on three types of oracles to ensure comprehensive coverage and data integrity:

#### 1. Technical Oracle
Software programmers who participate in mining as a side opportunity, typically handling **500,000 to 1 million properties**. These oracles:
- Possess technical expertise to run mining software and scripts
- Operate independently to earn MAHOUT tokens
- Often automate their mining operations for efficiency
- Contribute significant data volume to the network

#### 2. Institutional Oracle
Professional entities hired by organizations or high-net-worth individuals to mine MAHOUT on their behalf, managing **more than 1 million properties**. These oracles:
- Operate at enterprise scale with dedicated resources
- Provide white-glove mining services for clients
- Often have teams dedicated to different jurisdictions

#### 3. Owner/Provider Oracle
Non-technical individuals who benefit from the Elephant Protocol ecosystem, typically managing **1 to 100 properties**. These oracles:
- Include property owners verifying their own assets
- Service providers (agents, brokers) adding value for clients
- Community members contributing local property knowledge
- Use simplified tools and interfaces for participation

Each oracle type plays a crucial role in maintaining the integrity, timeliness, and verifiability of decentralized property records. The diversity of oracle participants ensures both comprehensive coverage and resistant to centralization.

### Oracle Mining Process Flow

```mermaid
graph LR
    A[Learn] --> B[Set Up]
    B --> C[Property Identification]
    C --> D[Mining]
    D --> E[Minting]
```

## Learning Key Concepts

Before beginning the oracle mining process, it's essential to understand the foundational concepts that power the Elephant Protocol.

### Lexicon Schema

The Lexicon is Elephant Protocol's domain-specific data format designed to create cross-jurisdictional consistency in property data. It serves as a universal translator between existing real estate data standards.

**Structure:**
- **Property Data Classes**: Core property attributes including ownership, assessments, and physical characteristics
- **Relationship Classes**: Define connections between properties, owners, and transactions - structured specifically for IPFS relationship management
- **Groups Structure**: Organized to optimize Merkle Tree construction for efficient on-chain verification

The Lexicon normalizes fragmented county data into a unified, queryable format while maintaining compatibility with existing systems. Each property's data is divided into distinct classes that can be independently verified and updated.

For detailed schema definitions and implementation guides, visit: [Elephant Protocol Lexicon](https://lexicon.elephant.xyz/)

### Oracle Wallets

Oracles receive MAHOUT and vMAHOUT tokens through crypto wallets, with two distinct approaches based on participant type:

**Individual Wallets**
- Standard crypto wallets like MetaMask for independent oracles
- Direct control over private keys
- Suitable for technical oracles and small-scale operations
- Simple setup with personal accountability

**Organization Wallets**
- Enterprise-grade wallet infrastructure leveraging Key Management Systems (KMS)
- Designed for institutional oracles managing large-scale operations
- Separates wallet control from individual employees
- Avoids the complicated processes required for traditional exchange-based corporate wallets
- Open-source framework ensures transparency and security

The wallet infrastructure ensures that both individual contributors and large organizations can participate in the oracle ecosystem while maintaining appropriate security and operational controls for their scale.

---

## Setup

Before you begin, ensure you have completed the following:

- **[Setup Guide](https://github.com/elephant-xyz/docs/blob/main/SETUP.md):** You must have a configured environment with a Google account, Pinata JWT, MetaMask wallet, and POL tokens.
- **[Tech Stack Review](https://github.com/elephant-xyz/docs/blob/main/TECH_STACK.md):** Familiarize yourself with the core technologies, including IPFS, IPLD, and JSON Canonicalization.

---
## Property Identification

The property identification process varies based on oracle type and scale of operations. This step establishes the foundational data needed to begin mining properties on the Elephant Protocol.

### Individual Property Identification

For oracles focusing on individual properties (Owner/Provider Oracles managing 1-100 properties):

1. **Locate Property Information**
   - Start with the property's street address
   - Google search: "[County Name] appraiser office" or "[County Name] assessor office"
   - Navigate to the county's property search portal

2. **Extract Core Identifiers**
   - Search for your target property using the address
   - Locate and record the **Parcel Identifier** (may also be called Folio ID, APN, or Parcel Number)
   - Verify the address matches county records

3. **Standardize Property Address**
   - Input the address into Google Maps
   - Use the formatted address provided by Google Maps as the standardized version
   - This ensures consistency across all data submissions

4. **Capture Source Information**
   - Copy the full URL of the property page from your browser
   - Open browser Developer Tools (F12)
   - Navigate to the Network tab
   - Refresh the property page
   - Identify the request method (typically GET)
   - Note any required headers or parameters for data access

### Large-Scale Property Identification

For Technical and Institutional Oracles managing thousands to millions of properties:

The Elephant Protocol foundational team has developed systematic approaches for collecting property identifiers across all 150 million properties in the United States. 

**Resources Available:**
- Pre-collected county data structures and access patterns
- Automated scripts for bulk property identification
- Standardized extraction methodologies by jurisdiction

View examples and access tools at: [US Properties](https://github.com/elephant-xyz/AI-Agent/tree/main/counties)

This repository contains:
- County-specific data schemas
- API endpoints and access methods
- Bulk extraction scripts
- Property identifier mapping tools

### Required Data Points

Regardless of scale, each property identification must include:
- **Standardized Address**: Google Maps formatted address
- **County**: Google Maps formatted address
- **Parcel Identifier**: The unique county-assigned property ID
- **Source URL**: Direct link to the county's property page
- **Request Method**: HTTP request method
- **Headers**: Additional HTTP request arguements

---
## Mining Process

The mining process transforms raw property data into validated Lexicon format ready for on-chain submission. The process varies based on which data group you're mining.

### Mining

```mermaid
graph LR
    A[Prepare] --> B[Enrich]
    B --> C[Transform]
    C --> D[Validate]
```

### Prepare

Input requirements vary by data group:

**Seed Group**
- Uses property identification fields prepared from the previous step
- Includes: Parcel ID, standardized address, source URL
- Minimal data requirements for initial property registration

**County Group**
- Start with property identification URL
- Download all HTML pages related to the property
- Extract all county-specific data and attributes
- Capture complete property record from county sources

**Photo Group**
- Locate publicly accessible URLs for property images
- Download all available property photos
- Store images for subsequent IPFS upload
- Maintain original image quality and metadata

**Photo Metadata Group**
- Download photos from IPFS (previously uploaded)
- Prepare images for AI Agent analysis
- Extract existing metadata from image files

### Enrich

AI-powered enrichment varies by data group:

**Not Required For:**
- Seed Group
- County Group  
- Photo Group

**Required For:**
- Photo Metadata Group

The AI Agent processes photos to gather detailed property information:
- Architectural features and building characteristics
- Property condition assessment
- Geolocation verification
- Environmental context and surroundings

### Transform

Converting raw data to Elephant Protocol's standardized schema:

**Simple Transformations:**
- **Seed**: Straightforward field mapping
- **Photo**: Basic metadata extraction
- **Method**: Manual mapping or simple scripts

**Complex Transformations:**
- **County**: Requires custom transformation code for each jurisdiction
- **Photo Metadata**: Converts AI Agent generated insights into Lexicon format
- **Method**: AI-aided transformation using [AI-Agent](https://github.com/elephant-xyz/AI-Agent)

The transformation complexity depends on:
- Data source structure
- Jurisdiction-specific formats
- Required field mappings
- Data validation rules

### Validate

The final validation step using the Elephant CLI:

- Validates data against Lexicon schema requirements
- Ensures all required fields are properly formatted
- Generates the final content hash for the data
- Enables off-chain validation before consensus submission
- Prepares data for on-chain commitment

The validation process:
1. Schema compliance check
2. Data integrity verification
3. Hash generation for consensus
4. Pre-submission verification

This step is critical for ensuring data quality and preventing failed consensus attempts.

**[Learn more about Seeding](https://github.com/elephant-xyz/docs/blob/main/1_SEEDING.md)**

## Minting

The minting process pushes all validated data to IPFS and commits the final hash to the blockchain, establishing an immutable property record.

### Step 5: Merkle Hash Commitment

**Objective:** Anchor the property's full dataset to the blockchain via Merkle root hash.

**Procedure:**

1. **IPFS Data Upload**
   - Push all data to IPFS including:
     - Raw JSON objects for property classes
     - Relationship classes
     - Group class data
   - Generate corresponding hashes in Merkle Tree format

2. **Merkle Tree Construction**
   - Build deterministic Merkle DAG from all uploaded data
   - Create hierarchical hash structure
   - Generate final Group hash representing all data

3. **Smart Contract Submission**
   - Submit the final Group hash to smart contract
   - CLI command `validate-and-upload` handles IPFS upload
   - CLI command `submit-to-contract` commits hash on-chain

4. **Consensus & Token Minting**
   
   **Groups Requiring Consensus (Seed and County):**
   - Requires 3 independent oracles to submit identical data
   - vMAHOUT minted when consensus reached
   - MAHOUT tokens will be minted (coming soon)
   - Consensus validates data accuracy across multiple sources

   **Groups Without Consensus (Photo and Photo Metadata):**
   - No consensus required
   - vMAHOUT minted immediately upon submission
   - MAHOUT tokens will be minted (coming soon)
   - Single oracle submission sufficient

**[Learn more about Merkle Hash Commitment](https://github.com/elephant-xyz/docs/blob/main/5_MERKLE_HASH_COMMITMENT.md)**

### Token Issuance

**Objective:** Reward oracle agents for successful contributions.

**Procedure:**

- Smart contract automatically mints tokens upon successful submission:
  - **vMAHOUT**: Non-transferable token attesting to verified contributions
  - **MAHOUT**: Transferable token representing economic and governance rights (coming soon)

- Oracle can view token status in their wallet
- Smart contract transactions viewable on Polygon explorer
- Tokens automatically distributed to oracle's registered wallet

**[Learn more about Token Issuance](https://github.com/elephant-xyz/docs/blob/main/6_TOKEN_ISSUANCE.md)**

### Verification

After minting process:
- Check wallet for vMAHOUT token receipt
- View smart contract status on Polygon explorer
- Verify IPFS hashes for data availability
- Monitor consensus status for applicable groups

This process creates an immutable, versioned record for the property while rewarding oracle contributions.

**Related Documentation:**
- **[Learn more about Consensus & Commitment](https://github.com/elephant-xyz/docs/blob/main/2_CONSENSUS_AND_COMMITMENT.md)**
---

## Reward Summary

| Stage        | Token Output | Description                          |
| ------------ | ------------ | ------------------------------------ |
| Mint Success | Mahout       | Economic token for staking/utility   |
| Mint Success | vMahout      | Proof-of-contribution (non-transfer) |

---

## Security & Consensus Guarantees

- Merkle DAG generation ensures data immutability and versioning.
- Multi-oracle validation mitigates bad data injection and single-point failure.
- Token rewards align oracle incentives with protocol data integrity.

---

## Future Extensions

- Integration of ZK proofs for ingestion verification.
- DAO-based oracle slashing for repeated invalid submissions.
- Automated jurisdictional prioritization via market demand signals.

---

## License

Copyright 2025 Elephant.xyz
Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License.
You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
