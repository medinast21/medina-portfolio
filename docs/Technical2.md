# Technical 2

## Authentication Bridge Service - Client Doc

### Overview
This document outlines the processes that both the client and Nelnet must complete in order to successfully authenticate using the Authentication
Bridge Service.

A successful authentication will provide the client access to the Velocity data APIs.

### What the client provides to Nelnet
* The public key from a public/private keyset created using the RSA algorithm that is ideally 4096 bits in size (typically the client can request this
from the client's IT department)
    * 4096 bits is the target size because larger key sizes will be processed more slowly by the system.
    * The public key created as part of the keyset should be given to Nelnet. Do not give Nelnet the private key.
* A JWT minted and signed with the private key from the keyset created that includes the client's Velocity BorrowerId value (GUID)
    * The client's BorrowerId value (GUID) is provided to them in the OnboardingId (GUID) bundle during the onboarding process