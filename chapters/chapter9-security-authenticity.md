# Chapter 9 — Security and Authenticity

## 9.1 Hash verification

### Document integrity

#### Principle
Each MML document can be accompanied by a cryptographic hash guaranteeing it hasn't been modified.

#### Format with hash
```
T:Official document
H:Section 1
P:Authenticated content
HASH:SHA256:a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3
```

#### Verification
```javascript
const crypto = require('crypto');

function verifyMMLIntegrity(mmlText) {
  const lines = mmlText.split('\n');
  const hashLine = lines.find(line => line.startsWith('HASH:'));

  if (!hashLine) return false;

  const expectedHash = hashLine.split(':')[2];
  const contentWithoutHash = lines.filter(line => !line.startsWith('HASH:')).join('\n');
  const calculatedHash = crypto.createHash('sha256').update(contentWithoutHash).digest('hex');

  return calculatedHash === expectedHash;
}
```

## 9.2 Lightweight signatures

### Source authentication

#### ED25519 for embedded
- **Key size**: 32 bytes
- **Signature**: 64 bytes
- **Fast verification**: Adapted to low resources

#### Signed format
```
T:Signed document
SIGN:ED25519:public_key_base64:signature_base64
[content]
```

### Trust chain

#### Lightweight certification authorities
- **Master keys** for organizations
- **Delegation** to authorized users
- **Revocation** by blacklist

## 9.3 Protection against malicious modifications

### Potential attacks

#### Content modification
- **Detection**: Invalid hash
- **Prevention**: Mandatory signature for critical documents

#### Fake fragment injection
- **Detection**: Source verification
- **Prevention**: Mutual authentication

#### Denial of service
- **Detection**: Reputation filtering
- **Prevention**: Rate limiting

### Protection measures

#### Optional encryption
For sensitive content:
```
ENCRYPT:AES256:key_id
[encrypted content in base64]
```

#### Secure channels
- **DNF over TLS**: When network available
- **End-to-end encryption**: Protection against interception

## 9.4 Anonymous transmission

### Privacy respect

#### Automatic anonymization
- **No personal identifier** in basic metadata
- **Relative timestamping** instead of absolute
- **Approximate coordinates** for protection

#### Anonymous format
```
T:Incident report
M:Type|Assault
M:Location|City center
M:Time|Last night
P:Incident reported anonymously
```

### Security/anonymity balance

#### Configurable level
- **Public**: No protection
- **Protected**: Automatic anonymization
- **Confidential**: Mandatory encryption

## 9.5 Security in humanitarian contexts

### Communication priority

#### Humanitarian principle
In crisis situations, **communication takes precedence over perfect security**.

#### Proportional security
- **Public documents**: No encryption
- **Sensitive information**: Light encryption
- **Medical data**: Minimal authentication

### Context-based authentication

#### Social verification
- **Multiple witnesses**: Corroboration
- **Transmission chain**: Human traceability
- **Operational context**: Validation by situation

### Key management

#### Embedded keys
- **Pre-installed** in humanitarian equipment
- **Automatic rotation**: Periodic
- **Recovery**: Backup procedures

---

**Chapter conclusion**: MML security is designed to be proportional to context. In humanitarian environments where lives are at stake, priority is given to reliable communication rather than perfect security that could block transmissions. MML offers "sufficient" and adaptable security, prioritizing operational efficiency over cryptographic perfection.

