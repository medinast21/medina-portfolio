# Technical 2

## Authentication Bridge Service - Client Doc

### Overview
This document outlines the processes that both the client and Nelnet must complete in order to successfully authenticate using the Authentication
Bridge Service.

A successful authentication will provide the client access to the Velocity data APIs.

#### What the client provides to Nelnet
* The public key from a public/private keyset created using the RSA algorithm that is ideally 4096 bits in size (typically the client can request this
from the client's IT department)
    * 4096 bits is the target size because larger key sizes will be processed more slowly by the system.
    * The public key created as part of the keyset should be given to Nelnet. Do not give Nelnet the private key.
* A JWT minted and signed with the private key from the keyset created that includes the client's Velocity BorrowerId value (GUID)
    * The client's BorrowerId value (GUID) is provided to them in the OnboardingId (GUID) bundle during the onboarding process

#### What Nelnet provides to the client
* Nelnet creates the authentication bridge records and sends the bridge key to the client.
* The bridge authentication endpoint URL: https://<ENV>.nelnet.io/authenticationbridgeapi/authentication-bridge/authenticate
    * Depending on the environment, replace the <ENV> with the value provided for a specific environment.

### Process requirements
1. The client must create a new public/private keyset using the RSA algorithm; this should be around 4096 bits in size.

2. The client sends the public key of the keyset to Nelnet.

![Image placeholder]()

3. Nelnet configures the authentication bridge and sends the bridge key to the client.
    * Example: 418281ac-22dc-4a37-a136-9c377a41da31

4. The client creates a new JWT that is signed with the private key generated above, using the payload formatting shown in the example here:

``` title="Public Key Example"
-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA5osWL21cwZ3LWjLTsx5T
O9YVb0S3EH3eh4XihJE5SUnalLFzs0ATE1Eaf3NiLeFzOV5uv3U+HudyyT2KCcKo
Aw5o2juUfsqDKM5OI1/AhzEwFmnX16LfCiMOJYhCMYNM55m3Cn7z/AmojMq2mbGE
CtGAaIGx+2zZlfVLIF4sv8ONvyMxyGmgiwvIjrk44AkwZQN6dZq0XkuZsbtVqZ8p
tC/MjPNavY7GuM8YL5OTYdcS6BgSDlGuo1zf7jEH5Qp54uFqrYH75x6llK7QasVe
ERpEULjdxymVB+cql8KqpAycTCC/mM0auOAn3fnktQ3Irt9HBUAGIYHHeGyWGHFb
6oIZTW3wrTaCZXDyaW7d9T0ZBE5bVts3NTCBh2geJbbctI4U52hWMLtgnuXtcS6l
GzBkjEcnZb4fNU9guYCJXnQoVe0KFrD2V+cLYlfgltmlp1C3+CUkJV1E+0XTjXex
lsW4VZ+wiZ3i8b9tYKBfhLhmc3/bpafnBl1RCNerl1eNuBHFGj6Px/y6KRMtf/03
FkMyz5tC/jV0a7LYLNTKedxuKeLphWUJnZYdZM2tnPy6u8aoQhEiHbNMny2awM0/
Ighfbyudd5m2xLSfA6gtlzS7oJm7cs6gCYZkv85i8ynndBHfoXdxb+coEO0WepJ2
LjMaeSIArpf6HZ1yVkjn/bsCAwEAAQ==
-----END PUBLIC KEY-----
```
