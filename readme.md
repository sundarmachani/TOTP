# Time-Based One-Time Password (TOTP) Generator

A Python implementation of TOTP authentication system using HMAC-SHA512 for generating secure, time-based passwords.

## Overview

This script implements a TOTP (Time-based One-Time Password) generator that creates temporary passwords based on:
- A shared secret (user email + suffix)
- Current timestamp
- HMAC-SHA512 hashing

## Features

- Generates 10-digit time-based passwords
- Uses HMAC-SHA512 for enhanced security
- 30-second time step for password rotation
- Combines user identifier with secret suffix
- Zero-padding for consistent password length

## How It Works

1. **Secret Generation**
```python
userid = "user@gmail.com"
secret_suffix = "ABCD"
shared_secret = userid + secret_suffix
```

2. **HOTP (HMAC-based One-Time Password)**
```python
def HOTP(K, C, digits=10):
    K_bytes = K.encode()
    C_bytes = struct.pack(">Q", C)
    hmac_sha512 = hmac.new(key=K_bytes, msg=C_bytes, digestmod=hashlib.sha512).hexdigest()
    return Truncate(hmac_sha512)[-digits:]
```

3. **TOTP Generation**
```python
def TOTP(K, digits=10, timeref=0, timestep=30):
    C = int(time.time() - timeref) // timestep
    return HOTP(K, C, digits=digits)
```

## Use Cases

- **Two-Factor Authentication (2FA)**: Generate temporary passwords for additional security layer
- **API Authentication**: Create time-based tokens for API access
- **Secure Systems**: Implement one-time password functionality in security systems
- **User Verification**: Temporary code generation for user verification

## Requirements

- Python 3.x
- Standard libraries: hmac, hashlib, time, struct

## Usage

1. Set your user email and secret suffix:
```python
userid = "your_email@domain.com"
secret_suffix = "YOUR_SECRET"
```

2. Run the script:
```bash
python totp_generator.py
```

## Security Considerations

- Keep the secret suffix private
- Use HTTPS for transmitting passwords
- Consider implementing rate limiting
- Regularly rotate secret suffixes
- Validate time synchronization between server and client

## Why TOTP?

1. **Security**: Time-based passwords provide better security than static passwords
2. **Convenience**: Automatic password rotation without user intervention
3. **Industry Standard**: Widely accepted 2FA method
4. **Stateless**: No need to store previous passwords
5. **Time-Limited**: Passwords automatically expire after the time step