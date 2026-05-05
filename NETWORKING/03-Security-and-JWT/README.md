# 🔐 Security and JWT

> **Why this matters:** The web is a hostile environment. Without security, attackers can steal data, impersonate users, and destroy systems. JWT is the standard for stateless authentication.

---

## 🧠 Mental Model: The Hotel Key Card System

Imagine you're at a hotel:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   YOU (User)              HOTEL (Server)         HOUSEKEEPING   │
│      │                        │                      │           │
│      │  1. Show passport       │                      │           │
│      │ ──────────────────────▶ │                      │           │
│      │                        │                      │           │
│      │  2. Here's your        │                      │           │
│      │     key card           │                      │           │
│      │ ◀─────────────────────│                      │           │
│      │                        │                      │           │
│      │  3. Access room 302    │                      │           │
│      │ ◀─────────────────────│ (key validates)       │           │
│      │                        │                      │           │
│      │  4. Room is open!      │                      │           │
│      │ ◀─────────────────────│                      │           │
│      │                        │                      │           │
└─────────────────────────────────────────────────────────────────┘
```

**JWT works the same way:**

1. You prove your identity (username + password)
2. Server gives you a **token** (key card)
3. You show the token on each request
4. Server validates it and grants access

**The key difference from sessions:** The key card contains ALL your info inside it. The hotel (server) doesn't need to remember you — the card proves who you are.

---

## 📦 JWT Structure: The Three Parts

A JWT has three parts separated by dots:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3OD  │
│   ───────────────┬─────────────────────┬─────────────────────   │
│                  │                     │                        │
│            HEADER (JSON)        PAYLOAD (JSON)        SIGNATURE │
│            Base64URL            Base64URL              (HMAC)    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Part 1: Header

```json
{
  "alg": "HS256",       // Algorithm used to sign
  "typ": "JWT"          // Type of token
}
```

### Part 2: Payload (The Claims)

```json
{
  "sub": "1234567890",   // Subject (usually user ID)
  "name": "Ana García",
  "role": "admin",
  "iat": 1704067200,    // Issued at (timestamp)
  "exp": 1704153600     // Expiration (1 day later)
}
```

### Part 3: Signature

```javascript
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret_key
)
// Creates: dGFtJIUzI1NiJ9.eyJzdWIiOiIxMjM...
```

---

## 🔍 JWT Flow: Full Authentication Cycle

### ASCII: Login + Protected Request

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   FRONTEND                     API                    DATABASE   │
│      │                          │                        │        │
│      │  POST /login             │                        │        │
│      │  { email, password }     │                        │        │
│      │ ────────────────────────▶│                        │        │
│      │                          │                        │        │
│      │                          │ (verify credentials)  │        │
│      │                          │◀──────────────────────│        │
│      │                          │                        │        │
│      │  { token: "eyJhbG..." } │                        │        │
│      │ ◀───────────────────────│                        │        │
│      │                          │                        │        │
│      │  ┌─────────────────────────────────────────┐   │        │
│      │  │  STORE TOKEN IN localStorage/cookies    │   │        │
│      │  └─────────────────────────────────────────┘   │        │
│      │                          │                        │        │
│      │  GET /api/orders         │                        │        │
│      │  Authorization: Bearer   │                        │        │
│      │  eyJhbG...              │                        │        │
│      │ ────────────────────────▶│                        │        │
│      │                          │                        │        │
│      │                          │ (verify token)         │        │
│      │                          │ (extract user ID)       │        │
│      │                          │                        │        │
│      │                          │ (fetch user's orders)  │        │
│      │                          │◀──────────────────────│        │
│      │                          │                        │        │
│      │  { orders: [...] }      │                        │        │
│      │ ◀───────────────────────│                        │        │
│      │                          │                        │        │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ JWT Security Best Practices

### ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Always verify the signature** | Ensure token wasn't tampered with |
| **Check expiration (`exp`)** | Tokens shouldn't last forever |
| **Use HTTPS always** | Tokens in transit must be encrypted |
| **Store securely** | httpOnly cookies > localStorage |
| **Use short expiration + refresh tokens** | Limits damage if token is stolen |
| **Include `jti` (JWT ID)** | Enables token revocation |
| **Use strong secret keys** | Minimum 256 bits for HS256 |

### Implementation Example

```javascript
// GENERATE TOKEN
const token = jwt.sign(
  {
    sub: user.id,
    email: user.email,
    role: user.role,
    jti: crypto.randomUUID()  // For revocation
  },
  process.env.JWT_SECRET,
  {
    expiresIn: '15m',  // Short-lived!
    issuer: 'my-app'
  }
);

// VERIFY TOKEN
const decoded = jwt.verify(token, process.env.JWT_SECRET, {
  issuer: 'my-app'  // Verify issuer
});

// REFRESH TOKEN (long-lived refresh vs short-lived access)
const refreshToken = jwt.sign(
  { sub: user.id, type: 'refresh' },
  process.env.JWT_REFRESH_SECRET,
  { expiresIn: '7d' }
);
```

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't store JWT in localStorage** | XSS attacks can steal it |
| **Don't put sensitive data in payload** | It's Base64-encoded, easily decoded |
| **Don't use `none` algorithm** | Attacker can forge tokens |
| **Don't trust unsigned tokens** | Anyone can create a fake token |
| **Don't set expiration too long** | Risk if token is stolen |
| **Don't forget to handle expiration errors** | `TokenExpiredError` = user must re-login |

---

## 🔄 Refresh Token Strategy

### The Two-Token Pattern

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   TOKEN TYPES:                                                   │
│                                                                  │
│   ┌──────────────────┐    ┌──────────────────┐                 │
│   │  ACCESS TOKEN    │    │  REFRESH TOKEN   │                 │
│   │                  │    │                  │                 │
│   │  Lifetime: 15min │    │  Lifetime: 7days │                 │
│   │  Contains:      │    │  Contains:       │                 │
│   │   - user ID    │    │   - user ID      │                 │
│   │   - role       │    │   - type: refresh │                 │
│   │                  │    │                  │                 │
│   │  Used for:     │    │  Stored in:       │                 │
│   │   - API calls  │    │   - DB (revocable)│                 │
│   │                  │    │                  │                 │
│   │  Stored in:     │    │  Used for:       │                 │
│   │   - memory     │    │   - getting new  │                 │
│   │   - httpOnly   │    │     access token │                 │
│   │     cookie     │    │                  │                 │
│   └──────────────────┘    └──────────────────┘                 │
│                                                                  │
│   BENEFIT: Even if access token is stolen, attacker has 15min.  │
│            Refresh token requires DB theft + access token.      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔐 OAuth 2.0: Delegated Authorization

### When to Use OAuth vs. JWT

| Scenario | Use |
|----------|-----|
| Your own API, you control everything | JWT |
| Third-party login (Google, GitHub) | OAuth 2.0 + JWT |
| API needs to act on behalf of user | OAuth 2.0 |
| Server-to-server communication | Client Credentials + JWT |

### ASCII: OAuth Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   USER            APP              GOOGLE            API          │
│      │              │                │                │          │
│      │  Click "Login│                │                │          │
│      │   with Google│                │                │          │
│      │ ────────────▶│                │                │          │
│      │              │                │                │          │
│      │              │ Redirect to    │                │          │
│      │              │────────────────▶│                │          │
│      │              │                │                │          │
│      │  Login page  │                │                │          │
│      │ ◀────────────│                │                │          │
│      │              │                │                │          │
│      │  User enters │                │                │          │
│      │  credentials │                │                │          │
│      │ ────────────▶│                │                │          │
│      │              │                │                │          │
│      │              │◀───────────────│ (validates)    │          │
│      │              │                │                │          │
│      │  Auth code   │                │                │          │
│      │ ◀────────────│                │                │          │
│      │              │                │                │          │
│      │  Exchange for │                │                │          │
│      │  tokens      │                │                │          │
│      │ ────────────▶│────────────────│                │          │
│      │              │◀───────────────│                │          │
│      │              │  Access token  │                │          │
│      │              │  + ID token    │                │          │
│      │              │                │                │          │
│      │              │  Use ID token  │                │          │
│      │              │  to create JWT │                │          │
│      │              │                │                │          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚨 Common Security Threats

| Threat | How It Works | Mitigation |
|--------|---------------|------------|
| **XSS** | Inject malicious scripts | Sanitize input, CSP headers |
| **CSRF** | Forge requests from other sites | SameSite cookies, CSRF tokens |
| **Token Theft** | Steal from localStorage | Use httpOnly cookies |
| **Replay Attack** | Reuse intercepted token | Short expiration, `jti` |
| **Brute Force** | Guess credentials | Rate limiting, account lockout |

---

[⬅️ Back to Parent](../README.md)
