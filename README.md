# Formal Analysis of [Your Protocol Name] Protocol using ProVerif

This repository contains the ProVerif scripts for the formal security analysis of the [Your Protocol Name] protocol. The analysis covers two main phases of the protocol: Registration and Authentication.

## Overview

The [Your Protocol Name] protocol is designed to [briefly state the main purpose of your protocol, e.g., "enable secure communication and mutual authentication between users and multiple services in a distributed environment"]. This formal analysis aims to verify key security properties under the Dolev-Yao attacker model, which assumes the attacker has full control over the network.

## Tool Used

- **ProVerif:** An automatic cryptographic protocol verifier. More information can be found on the [ProVerif website](https://proverif.inria.fr/).

## Repository Structure

The repository includes the following ProVerif (`.pv`) files:

- `Registration.pv`: Models the registration phase where entities (Users/Servers) establish their identities and credentials with an Admin Server. This phase is assumed to occur over a secure (private) channel.
- `Authentication.pv`: Models the authentication phase where a User authenticates to one or more Servers (Si, Sj) potentially involving interaction with an Admin Server. This phase largely occurs over public channels.

## Security Properties Verified

The ProVerif scripts check for the following crucial security properties:

1.  **Secrecy:**
    - Confidentiality of long-term secret keys (e.g., `skE_actual`, `skAS_actual`, `skU_actual`, `skSi_actual`, `skSj_actual`).
    - Confidentiality of session-specific secrets generated during the protocol execution (e.g., `state_u`, `state_si`, `C_challenge`).
2.  **Authentication (Correspondence):**
    - Ensuring that when one party (e.g., a server) believes it has successfully authenticated another party (e.g., a user), the latter party indeed actively participated in the protocol with the intention to authenticate.
    - Verified for:
      - Admin Server completing registration initiated by an Entity.
      - Server Si authenticating User U.
      - Server Sj authenticating User U (in the context of communication involving Si).
3.  **Integrity (Injectivity):**
    - Ensuring a one-to-one mapping between corresponding protocol events (e.g., an authentication completion event at a server uniquely corresponds to an authentication initiation event by a user). This helps protect against certain forms of replay and impersonation attacks.

## Modeling Assumptions

Key assumptions made in the ProVerif models include:

- **Ideal Cryptography:** Cryptographic primitives (hashing, signatures, encryption) are assumed to be perfect (e.g., no algorithmic weaknesses).
- **Dolev-Yao Attacker:** The attacker can intercept, modify, inject, and delete messages on public channels but cannot break ideal cryptographic primitives without the necessary keys.
- **Private Channel for Registration:** The `Registration.pv` model assumes a secure (confidential and authenticated) channel between the Entity and the Admin Server (denoted `E_AS: channel [private]`).
- **Abstracted Operations:** Certain operations, like exponentiation (`pow`), are modeled as uninterpreted functions with their defined properties. Timestamp freshness checks and out-of-band verifications mentioned in protocol descriptions are generally abstracted and not explicitly modeled unless directly influencing message flow or cryptographic operations.
- **Pre-shared Keys/Trust:** The existence and secure pre-distribution of certain public keys or trust relationships are assumed where necessary (e.g., servers knowing public keys of other trusted servers or the Admin Server).

## How to Run the Scripts

To verify the protocols using ProVerif:

1.  Ensure ProVerif is installed on your system.
2.  Navigate to the directory containing the `.pv` files.
3.  Run ProVerif from the command line:

    ```bash
    proverif Registration.pv
    proverif Authentication.pv
    ```

4.  Examine the output. ProVerif will indicate whether the queried properties hold (`RESULT ... is true`) or if it found a potential attack trace.

## Disclaimer

This formal analysis provides a level of assurance regarding the specified security properties within the defined model. It does not cover all possible security aspects, such as side-channel attacks, denial-of-service vulnerabilities not related to logical flaws, or implementation errors.
