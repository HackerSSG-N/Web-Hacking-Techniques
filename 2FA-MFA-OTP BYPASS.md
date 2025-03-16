# 2FA Bypass Techniques

## Direct Endpoint Access

### Example:
- Suppose `https://example.com/login` redirects to `https://example.com/2fa`, and after successful 2FA, the user lands on `https://example.com/dashboard`.
    - If `https://example.com/dashboard` is accessible without completing 2FA, an attacker can navigate there directly.
    - If blocked, modifying the `Referer` header to `https://example.com/2fa` might trick the system.

## Token Reuse

### Example:
- If a one-time token like `123456` works multiple times instead of expiring after use, an attacker could replay a previously used token.

## Utilization of Unused Tokens

### Example:
- If a system allows extracting a token from a secondary account (e.g., an admin account) and reusing it in another, the attacker can bypass 2FA.

## Exposure of Token

### Example:
- If the 2FA token appears in a response payload (e.g., API returning `{ "2fa_token": "654321" }`), an attacker can intercept it using a proxy tool like Burp Suite.

## Verification Link Exploitation

### Example:
- If a system allows login via an email verification link (`https://example.com/verify?token=abc123`), and it does not enforce 2FA, an attacker could use the link to bypass it.

## Session Manipulation

### Example:
- Logging into both a legitimate account and a victim's account and swapping session cookies might allow bypassing 2FA.

## Password Reset Mechanism

### Example:
- If resetting a password automatically logs in the user without 2FA (`https://example.com/reset-password?token=xyz`), an attacker could reset a victim’s password and log in without 2FA.

## OAuth Platform Compromise

### Example:
- If a user logs in via Google OAuth (Sign in with Google), compromising their Google account allows access without dealing with 2FA on the target site.

## Brute Force Attacks

### Rate Limit Absence

#### Example:
- If OTP input has no attempt limits, an attacker could brute-force the correct OTP (`000000` to `999999`).

### Slow Brute Force

#### Example:
- Sending one attempt every 10 minutes avoids triggering rate limits.

### Code Resend Limit Reset

#### Example:
- If each code resend resets the attempt counter, an attacker can exploit this by continuously resending OTPs.

### Client-Side Rate Limit Circumvention

#### Example:
- If rate limits are enforced client-side via JavaScript, disabling JavaScript bypasses them.

### Internal Actions Lack Rate Limit

#### Example:
- If OTP brute-force attempts are blocked, but the verification API (`/verify-otp`) does not enforce rate limits, an attacker could exploit this.

### SMS Code Resend Costs

#### Example:
- Spamming the OTP resend function drains a company's SMS budget, potentially causing a DoS attack.

### Infinite OTP Regeneration

#### Example:
- If generating a new OTP invalidates the old one, an attacker can brute-force a small number of possible OTPs (`0000` to `9999`) while continuously regenerating them.

### Race Condition Exploitation

#### Example:
- Sending multiple OTP verification requests in quick succession might allow bypassing rate limits or OTP expiration checks.

## CSRF/Clickjacking Vulnerabilities

### Example:
- If an attacker tricks a user into clicking a hidden button that disables 2FA, it can be bypassed.

## "Remember Me" Feature Exploits

### Predictable Cookie Values

#### Example:
- If `remember_me` cookies contain predictable values (`Base64(user_id)`), an attacker can forge them.

### IP Address Impersonation

#### Example:
- If IP-based 2FA bypasses verification (`X-Forwarded-For: victim-ip`), an attacker can spoof it.

## Utilizing Older Versions

### Subdomains

#### Example:
- If `old.example.com` lacks 2FA but shares authentication with `example.com`, an attacker could log in via `old.example.com` and gain access.

### API Endpoints

#### Example:
- `https://api.example.com/v1/auth` might enforce 2FA, but `https://api.example.com/v0/auth` might not.

### Handling of Previous Sessions

#### Example:
- If a user is still logged in on another device after enabling 2FA, an attacker who controls that session bypasses 2FA.

## Access Control Flaws with Backup Codes

### Example:
- If an attacker can request backup codes without re-authentication, they can use them to bypass 2FA.

## Information Disclosure on 2FA Page

### Example:
- Displaying a partially masked phone number (`+1-XXX-XXX-1234`) helps attackers identify the victim’s carrier for targeted attacks.

## Password Reset Disabling 2FA

### Example:
- If resetting a password disables 2FA, an attacker can exploit this by resetting a victim’s password.

## Decoy Requests

### Example:
- Sending decoy requests to security mechanisms (e.g., fake logins) to mask actual brute-force attempts.

## OTP Construction Errors

### Example:
- If OTPs are derived from timestamps or user data (`OTP = hash(username + timestamp)`), an attacker can predict them.
