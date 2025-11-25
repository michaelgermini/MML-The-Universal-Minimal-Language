# Appendix B — Complete Commented MML Document Example

## Complete MML document with comments

```
# Complete MML example document
# This document demonstrates all main tags

T:Complete MML Language Guide
M:Author|MML Team
M:Version|1.0
M:Date|2025-11-15
M:License|CC-BY-SA

H:Introduction to MML

P:The Minimal Markup Language (MML) is a markup format designed for transmission and archiving of structured documents in constrained environments.

P:Its minimalist syntax makes it particularly suited to situations where communication must remain possible despite technical or environmental limitations.

Q:"Simplicity is the ultimate sophistication" - Leonardo da Vinci

H:Basic syntax

P:MML syntax is based on simple principles:

P:1. Each line contains a unique element
P:2. Format is TAG:content
P:3. Tags are short and mnemonic

C:# JavaScript code example
C:function parseMML(text) {
C:  return text.split('\n')
C:    .filter(line => line.includes(':'))
C:    .map(line => {
C:      const [tag, content] = line.split(':', 2);
C:      return { tag, content: content.trim() };
C:    });
C:}

H:Transmission and resilience

P:MML can be transmitted through many channels:

L:Morse transmission|../chapters/chapter6-mml-transmission.md#65-transmission-via-morse
L:DNF networks|../chapters/chapter7-mml-dnf.md
L:Amateur radio|../chapters/chapter6-mml-transmission.md#66-transmission-via-amateur-radio

IMG:Transmission diagram|transmission-schema.png

H:Comparative advantages

P:Compared to traditional formats, MML offers:

M:Simplicity|5-minute learning syntax
M:Resilience|Tolerates partial losses
M:Universality|Transmittable by any means
M:Efficiency|High signal-to-noise ratio

H:Use cases

P:MML finds applications in many domains:

H:Conflict zones
P:Coordination of rescue operations despite infrastructure destruction.

H:Natural disasters
P:Transmission of vital information in isolated areas.

H:Amateur radio
P:Digital communication in radio band.

H:Embedded applications
P:Technical documentation on low-resource systems.

H:Implementations

P:MML is implemented in several languages:

CFG:JavaScript
M:Parser|Complete with DOM
M:Converters|HTML, JSON, text
M:Size|15kb minified

CFG:Python
M:Library|mml-parser
M:Features|Parsing + validation
M:Dependencies|None

CFG:C (embedded)
M:Library|LibMML
M:Memory|256 bytes RAM
M:Flash|2kb

H:Security

P:MML supports several security mechanisms:

M:Hash|SHA256 for integrity
M:Signature|ED25519 for authentication
M:Encryption|AES256 optional

HASH:SHA256:this-is-an-example-hash-fingerprint-for-demonstration

H:Ecosystem and future

P:The MML ecosystem includes:

L:DNF - Transport protocol|../chapters/chapter7-mml-dnf.md
L:MMLC - Compressed version|../chapters/chapter5-mmlc-compression.md
L:Converters|../chapters/chapter8-mml-converters.md

P:MML's future is oriented towards:
P:- International humanitarian adoption
P:- Embedded AI integration
P:- Controlled extensions (MML 2.0)

Q:MML represents a minimalist and resilient approach to digital communication, adapted to the challenges of the 21st century.
```

## Document analysis

### Hierarchical structure
- **Level 0**: Main title (T:)
- **Level 1**: Main sections (H:)
- **Level 2**: Subsections (H: under H:)
- **Content**: Paragraphs (P:), quotes (Q:), code (C:)

### Metadata
- Documentary information (author, version, date)
- Implementation configuration
- Security (hash)

### Rich content
- Formatted text with important quotes
- Code blocks with implicit syntax highlighting
- Links to other sections
- Referenced images
- Structured metadata

### Transmission
This document can be:
- **Dictated in Morse** for radio transmission
- **Fragmented into DNF packets** for resilience
- **Compressed to MMLC** for savings
- **Automatically converted** to HTML/JSON/PDF

---

**This example document demonstrates the expressive richness of MML despite its minimalist syntax.**

