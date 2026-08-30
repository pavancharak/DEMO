# PARMANA
## Proof of Authorization for AI Execution

**Cryptographic authorization layer for regulated AI agents. Sign transactions. Prove consent. Detect fraud.**

---

## THE PROBLEM

### Why This Matters

Today, AI agents execute financial transactions on behalf of enterprises. But there's a fatal gap in the security model:

**Initial Permission ≠ Runtime Proof of Authorization**

- A developer grants an AI agent access to move money
- The agent's credentials get compromised (via supply-chain attack, misconfiguration, or prompt injection)
- The hacker now controls the agent and moves funds **the customer never authorized**
- The transaction clears because the agent "had permission"
- No audit trail proving the customer actually authorized this specific action
- Enterprise is liable

### Real-World Incident

**July 2026 — OpenAI Supply Chain Attack**
- Compromised internal models coordinated through HuggingFace
- Unauthorized execution of commands via runtime authority manipulation
- Proof: Authorization was not re-validated at execution time

**August 2026 — UK AI Security Institute**
- Agentic supply-chain attack using fake identity credentials
- Initial permission did not equal runtime authorization
- **Lesson:** You need cryptographic proof of each action, not just initial access grants

---

## THE SOLUTION

### What Parmana Does

Parmana is an authorization layer that sits in front of execution. Before an AI agent executes any transaction:

1. **Issue a Cryptographic Challenge**
   - Agent must sign the action with a device-bound key
   
2. **Verify Proof of Authorization**
   - System validates: "Did the key holder (human/trusted system) actually authorize THIS action?"
   
3. **Execute Only if Verified**
   - Signed actions proceed; unsigned or forged actions are rejected
   
4. **Audit Trail**
   - Every action has cryptographic proof it was authorized
   - Unforgeable. Tamper-evident. Auditable.

### How It Works

```
┌─────────────────────┐
│  Customer Issues    │
│   Authorization     │
│   "Send ₹50K to X"  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Sign with Device  │
│   Cryptographic Key │
│   (Hardware Keystore)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Authorization      │
│  Proof Created      │
│  (Unforgeable)      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Agent Executes    │
│   ONLY if Proof     │
│   is Valid          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Audit Trail:       │
│  "Pavan signed this"│
│  Timestamp + Proof  │
└─────────────────────┘
```

---

## THE VALUE

### For Regulated Enterprises

| Requirement | Parmana Solution |
|---|---|
| **Proof of authorization** | Every action signed cryptographically |
| **Audit trail** | Unforgeable record of who authorized what |
| **Compliance** | Demonstrates runtime authorization (not just initial access) |
| **Fraud detection** | Tampered signatures are instantly rejected |
| **Customer trust** | "Your authorization is cryptographic. We can't override it." |

### For Financial Services (Banks, Fintechs)

- **RBI/FCA Requirements:** Proof that transactions are authorized by the account holder, not just by the system
- **Parmana:** Provides that proof at the action level, not the access level
- **Liability:** Enterprise can prove "yes, the customer signed this exact action"

### For Payment Networks

- **Chargeback Risk:** "I never authorized that ₹50K transfer"
- **Parmana:** Cryptographic proof the customer did authorize it (signature verification)
- **Settlement:** Disputes resolved faster with unforgeable evidence

---

## THE DEMO

### What You'll See

A 3-screen interactive walkthrough showing authorization, signing, proof, and forgery detection.

**Screen 1: Authorize**
- User describes what they want to authorize
- Example: "Send ₹50,000 to Vendor ABC"

**Screen 2: Unlock**
- Simulated biometric verification
- Device generates cryptographic signature

**Screen 3: Proof**
- Shows the signed authorization
- User can verify the signature is valid ✓
- User can attempt to forge/tamper with the signature
- Forgery attempt is instantly detected and rejected ✗

### Why This Demo Matters

The **"Try to Forge It" moment** is the wow factor:

1. Change one character in the signature
2. Verification fails: ✗ **Forgery Detected**
3. User realizes: "This is unbreakable. Even hackers can't fake this."

**That's the value prop in action.**

---

## HOW TO RUN THE DEMO

### Quick Start

1. **Download** `parmana-demo-v2.html`
2. **Double-click** the file (opens in browser)
3. **Tap through** the 3-step flow (90 seconds)

### System Requirements

- Any modern web browser (Chrome, Safari, Firefox, Edge)
- No internet required (fully offline)
- Works on desktop, tablet, mobile

### Technical Details

**Cryptography Used:**
- RSA-PSS 2048-bit for signing
- SHA-256 hashing
- Web Crypto API (native browser cryptography)

**Private Key Security:**
- Generated fresh in browser memory
- Never transmitted, exported, or logged
- Hardware Keystore simulation (production uses Android/iOS/TPM)

---

## THE 90-SECOND DEMO SCRIPT

### Setup
Open the demo file in any browser. Phone frame shows on screen. Projector friendly (420px width).

### Narration

```
[Welcome screen shows]
"Here's Parmana. The problem: hackers steal AI credentials 
and move money you never authorized. 

Parmana fixes this with cryptographic proof.
Let me show you how it works."

[Tap "See the Demo"]

[Authorization screen shows]
"Step 1: What do you want to authorize?
I'm going to send ₹50,000 to Vendor ABC. 
This is a real transaction instruction."

[Enter message or keep default]
[Tap "Sign Authorization"]

[Biometric screen shows]
"Step 2: Unlock with your fingerprint.
Your device generates a cryptographic signature. 
This signature proves YOU authorized this exact action."

[Tap "Use Fingerprint"]

[Proof screen appears]
"Step 3: Here's your proof.

This is the message. This is the signature. 
Cryptographic proof you authorized this.

Nobody can fake this. Not even hackers with admin access.
Let me show you."

[Tap "✓ Verify Signature"]
→ "Signature verified! This proves YOU authorized this."

[Tap "Try to Forge It"]
→ "Someone tries to modify the signature..."

[Tap "Verify Signature" again]
→ "✗ Forgery detected! IMPOSSIBLE to fake."

[Close demo]
"That's Parmana. 

Authority is cryptographic.
Only what you authorize becomes real.

For AI agents moving money, for payment networks, 
for any regulated system that needs proof.

Questions?"
```

---

## ARCHITECTURE

### Three-Layer Model

**Layer 1: Authorization**
- User/system describes action
- Action stored in plaintext

**Layer 2: Signing**
- Device-bound cryptographic key signs the action
- Signature is mathematically unforgeable
- Private key never leaves secure enclave

**Layer 3: Verification**
- Before execution, system verifies signature
- If tampered or invalid: reject
- If valid: execute and audit

### Secure Key Management

**Production (Real System):**
- Android Keystore (hardware-backed on Pixel 6+)
- Apple Secure Enclave (iPhone/Mac)
- TPM 2.0 (enterprise servers)
- Hardware Security Module (HSM)

**Demo (Browser):**
- Web Crypto API (simulated)
- Ed25519 / RSA-PSS
- Generated in memory (not persistent)

---

## USE CASES

### Financial Services

**Problem:** Customer claims "I never authorized that payment"
**Parmana Solution:** Bank shows cryptographic proof customer signed it

**Example:** ₹50,000 transfer to vendor
- Customer authorizes: "Send ₹50K to Acme Corp"
- Device signs: cryptographic proof
- Bank executes: with unforgeable audit trail
- Dispute: Bank shows signature verification, dispute closed

### Regulated AI Agents

**Problem:** AI agent gets hacked, moves money without permission
**Parmana Solution:** Every agent action must be signed and verified

**Example:** AI procurement agent
- Agent identifies vendor invoice ₹100K
- Agent **proposes**: "Pay this invoice"
- System **challenges**: "Sign this payment"
- Procurement manager signs authorization
- Agent executes **only** with valid signature
- Attacker can't bypass: would need signature

### Payment Networks

**Problem:** Chargebacks due to "unauthorized" claims
**Parmana Solution:** Unforgeable proof of authorization

**Example:** Razorpay/Stripe transaction
- Merchant requests payment
- Customer authorizes with fingerprint
- Payment signed with device key
- Settlement: merchant has cryptographic proof
- Chargeback defense: show signature verification

---

## COMPETITIVE ADVANTAGES

| Feature | Parmana | Traditional OAuth | Traditional 2FA | MFA Alone |
|---|---|---|---|---|
| **Proof of specific action** | ✓ | ✗ | ✗ | ✗ |
| **Cryptographic signature** | ✓ | ✗ | ✗ | ✗ |
| **Audit trail per transaction** | ✓ | ✗ | ✗ | ✗ |
| **Unbreakable (mathematically)** | ✓ | ✗ | ✗ | ✗ |
| **Works offline** | ✓ | ✗ | ✗ | ✓ |
| **Device-bound** | ✓ | ✗ | ✓ | ✓ |

**Key Difference:** Parmana proves **"YOU authorized THIS action"** not just **"YOU logged in once"**

---

## MARKET OPPORTUNITY

### Regulated Sectors Requiring Proof of Authorization

- **Financial Services:** RBI, FCA, SEBI mandates runtime authorization
- **Healthcare:** HIPAA, GDPR require audit trails of who authorized what
- **Government:** GAO compliance for sensitive system access
- **Enterprise:** SOX, ISO 27001 require unforgeable authorization records

### Growing Use Case: Agentic AI

- Agencies are automating financial transactions
- Regulators require proof of authorization
- Parmana is **the only solution** that provides cryptographic proof at action level

---

## TECHNICAL SPECIFICATIONS

### Cryptographic Primitives

**Signing Algorithm:** RSA-PSS 2048-bit or Ed25519
**Hash Function:** SHA-256
**Key Generation:** NIST-approved curves
**Key Storage:** Hardware Keystore (production) / Web Crypto (demo)

### API (Production)

```
// Issue authorization challenge
const challenge = parmana.createChallenge({
  action: "Transfer ₹50000 to VendorABC",
  timestamp: Date.now(),
  nonce: crypto.randomUUID()
});

// Sign authorization (requires biometric/2FA)
const proof = await device.sign(challenge);

// Verify before execution
const isValid = parmana.verify(proof);

// Execute only if verified
if (isValid) {
  executeAction(challenge);
  auditLog.record(proof);
} else {
  reject("Unauthorized");
}
```

---

## WHY THIS MATTERS NOW

### Recent Events (2026)

1. **OpenAI Supply Chain Attack (July)** — Proves runtime authority is broken
2. **UK AISI Agentic Attack (Aug)** — Fake credentials bypass access controls
3. **Regulatory Pressure** — RBI, FCA asking: "How do you prove authorization?"

### The Gap Parmana Fills

- Existing solutions (OAuth, 2FA, MFA) prove **access** ✓
- **None** prove **authorization of a specific action** ✗
- Parmana fills that gap with cryptographic proof ✓

---

## THE ASK

### For Hackathon Judges

Parmana solves a **critical security gap** in regulated AI systems:

1. **Real Problem:** Initial access ≠ runtime authorization (proven by recent attacks)
2. **Clear Solution:** Cryptographic proof at action level
3. **Huge Market:** All regulated sectors + agentic AI explosion
4. **Defensible:** Unforgeable, auditable, cryptographically proven

### For Users/Enterprises

Parmana lets you:
- **Prove** customers authorized every action
- **Defend** against chargebacks and disputes
- **Comply** with RBI/FCA/GDPR requirements
- **Trust** AI agents with money

---

## NEXT STEPS

### From Demo to Product

1. **Phase 1 (Sept 2026):** FCA Supercharged Sandbox (proof of concept with UK fintech)
2. **Phase 2 (Oct 2026):** NFRA partnership (RBI regulatory guidance)
3. **Phase 3 (Q4 2026):** Production SDK for payment networks + banks
4. **Phase 4 (2027):** Enterprise AI agent authorization platform

---

## QUESTIONS?

**What is Parmana?**
Cryptographic proof of authorization for financial transactions and regulated AI agents.

**How is it different from 2FA/MFA?**
2FA proves identity. Parmana proves **authorization of a specific action.** Both needed.

**Is it secure?**
Yes. RSA-2048 and Ed25519 are NIST-approved. Private key never leaves device. Mathematically unforgeable.

**Can hackers bypass it?**
No. Would need to forge a cryptographic signature (computationally impossible) or steal the private key (stored in hardware secure enclave).

**Can I use this today?**
Yes. Demo is live. Production SDKs launching September 2026.

**Who needs this?**
Banks, fintechs, payment networks, enterprises running AI agents, healthcare, government — anyone moving money or sensitive data that requires proof of authorization.

---

## RESOURCES

- **Website:** parmanasystems.com
- **GitHub:** github.com/parmanasystems
- **Docs:** docs.parmana.io
- **Contact:** founder@parmanasystems.com

---

**Parmana: Authority is cryptographic. Only what you authorize becomes real.**

*Built for the 8x Mobile Hackathon, Gurugram. August 30, 2026.*
