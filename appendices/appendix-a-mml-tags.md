# Appendix A — Complete MML Tags Table

## Fundamental tags

| Tag | Name | Description | Example |
|-----|------|-------------|---------|
| `T:` | Title | Main document title | `T:Emergency report` |
| `H:` | Header | Section or header | `H:Introduction` |
| `P:` | Paragraph | Text paragraph | `P:This is a paragraph` |
| `L:` | Link | Hypertext link | `L:Documentation|https://example.com` |
| `IMG:` | Image | Image reference | `IMG:Sector map|map.png` |
| `C:` | Code | Code block | `C:console.log('Hello')` |
| `Q:` | Quote | Quote or important note | `Q:Warning: dangerous area` |
| `M:` | Metadata | Key-value metadata | `M:Author|John Doe` |

## Specialized tags

| Tag | Name | Description | Example |
|-----|------|-------------|---------|
| `CFG:` | Configuration | Configuration section | `CFG:Network settings` |
| `PKT:` | Packet | DNF container | `PKT:MSG-001` |

## Security tags (optional)

| Tag | Name | Description | Example |
|-----|------|-------------|---------|
| `HASH:` | Hash | Cryptographic hash | `HASH:SHA256:abc123...` |
| `SIGN:` | Signature | Digital signature | `SIGN:ED25519:key:signature` |
| `ENCRYPT:` | Encryption | Encrypted content | `ENCRYPT:AES256:key_id` |

## Detailed syntax

### General format
```
TAG:content
```

**Strict rules:**
- One TAG per line
- Separator `:` mandatory
- No spaces around `:`
- UTF-8 encoding

### Special content

#### Links (L:)
```
L:Display text|URL or reference
```

#### Metadata (M:)
```
M:Key|Value
```

#### Images (IMG:)
```
IMG:Image description|URL or path
```

### Reserved characters

| Character | Usage | Escape |
|-----------|-------|--------|
| `:` | TAG/content separator | `:text:` → `:text:` in content |
| `\|` | Separator in M:, L: | `\|` → `\|` in text |
| `\n` | End of line | Implicit |

## Proposed extensions (MML 2.0)

### Multimedia tags
- `AUDIO:` - Audio references
- `VIDEO:` - Video content
- `3D:` - 3D models

### Structural tags
- `CAL:` - Calendar events
- `TASK:` - Task management
- `FORM:` - Forms

### AI tags
- `AI:` - AI analysis metadata
- `ML:` - Embedded models
- `RULE:` - Validation rules

---

**Note**: This table defines the MML 1.0 standard. Extensions are proposed for future versions but remain backward compatible.

