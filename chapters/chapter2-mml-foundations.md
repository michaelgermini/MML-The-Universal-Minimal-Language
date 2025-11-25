# Chapter 2 — MML Foundations

## 2.1 Formal definition of MML

### Academic definition

**MML (Minimal Markup Language)** is a declarative markup language designed for serialization, transmission and archiving of structured documents in constrained environments.

**Formal properties**:
- **L1 type language**: Linear and sequential
- **Regular grammar**: Parsable by finite automata
- **Declarative semantics**: Focus on "what" rather than "how"
- **Error tolerance**: Partial operation possible

### Practical definition

**MML is a text format where each line represents a content element preceded by a short tag.**

```
T:Document title
H:Main section
P:Content paragraph
```

**Conceptual equivalent**:
```xml
<document>
  <title>Document title</title>
  <section>Main section</section>
  <paragraph>Content paragraph</paragraph>
</document>
```

## 2.2 Main objectives: universality, robustness, simplicity

### Universality as central objective

#### Transmission universality

MML must be transmittable by **all imaginable means**:

1. **Electronic transmission**:
   - Computer networks (HTTP, Bluetooth, digital radio)
   - File storage (disks, memory cards)

2. **Human transmission**:
   - Oral dictation (radio, telephone)
   - Morse code (telegraph, light, sound)
   - Manual writing (paper, blackboards)

3. **Mechanical transmission**:
   - Light signals (lamps, lasers)
   - Sound signals (bells, whistles)
   - Physical signals (flags, semaphores)

#### Interpretation universality

MML must be **understandable by**:
- **Humans**: Literate readers without technical training
- **Machines**: Simple parsers (microcontrollers, old computers)
- **Hybrid systems**: Human-machine combinations

### Robustness as reliability guarantee

#### Structural robustness

**Principle**: An MML document remains partially usable even when damaged.

**Resilience example**:

Original document:
```
T:Weather report
H:Forecast
M:Temperature|25°C
M:Wind|15 km/h
P:Fine weather expected
```

Damaged document (mixed lines, corrupted characters):
```
M:Temperature|25°C
H:Forecast
P:Fine weather expected
M:Wind|1? km/h
T:Weather report
```

**Result**: Essential information is preserved despite damage.

#### Temporal robustness

**Principle**: An MML document created today must remain readable in 50 years.

**Guarantees**:
- Stable and minimalist syntax
- No external dependencies
- Universal text encoding (UTF-8)
- Clear and lasting semantics

### Simplicity as strategy

#### Learning simplicity

**Learning curve**:
- **5 minutes**: Understand basic syntax
- **1 hour**: Master all tags
- **1 day**: Create complex documents

#### Implementation simplicity

**Minimal MML parser in Python**:
```python
def parse_mml(text):
    lines = text.strip().split('\n')
    document = {'nodes': []}

    for line in lines:
        if ':' in line:
            tag, content = line.split(':', 1)
            document['nodes'].append({
                'tag': tag.strip(),
                'content': content.strip()
            })

    return document
```

**Minimal MML parser in JavaScript**:
```javascript
function parseMML(text) {
  return text.trim().split('\n')
    .filter(line => line.includes(':'))
    .map(line => {
      const [tag, content] = line.split(':', 2);
      return { tag: tag.trim(), content: content.trim() };
    });
}
```

## 2.3 MML as serialization language

### Serialization vs other approaches

#### Comparison with binary serialization

**Advantages of binary serialization**:
- Compact size
- Processing speed
- Fixed and predictable structure

**Advantages of MML serialization**:
- Human readability
- Resilience to corruption
- Transmission via text channels
- Easy debugging

#### Concrete example: System configuration

**Binary format**: 45 bytes, unreadable
**JSON format**: 128 characters, verbose
**MML format**: 67 characters, readable

```
CFG:Network interface
M:IP|192.168.1.100
M:Mask|255.255.255.0
M:Gateway|192.168.1.1
M:DNS|8.8.8.8
```

### Serialization applications

#### 1. Equipment configuration
```
CFG:WiFi router
M:SSID|SecureNetwork
M:Password|ChangeRequired
M:Channel|6
M:Encryption|WPA3
```

#### 2. IoT sensor data
```
PKT:Temperature sensor
M:ID|sensor_01
M:Value|23.5
M:Unit|Celsius
M:Timestamp|2025-11-15T14:30:00Z
```

#### 3. System messages
```
T:System alert
M:Level|CRITICAL
M:Component|Database
M:Message|Connection lost
M:Action|Automatic restart
```

## 2.4 Why a strict but readable format?

### The delicate balance

#### Too strict: Problems with rigid formats

**JSON example**:
```json
{
  "title": "Document",
  "content": "Text"
}
```

**Problems**:
- Inability to add comments
- Mandatory commas
- Quotes everywhere
- Fixed tree structure

#### Too loose: Problems with free formats

**Markdown example**:
```
# Title

Paragraph with **bold** and *italic*.

- List 1
- List 2
```

**Problems**:
- Interpretation ambiguities
- Multiple incompatible extensions
- Complex parsers
- Limited resilience

#### MML approach: Strict but human

**Principle**: Clear, minimal rules, but sufficient to avoid any ambiguity.

**Strict MML rules**:
1. One tag per line
2. Mandatory `:` separator
3. No complex nesting
4. UTF-8 encoding

**Advantages**:
- Deterministic parsing
- Maximum resilience
- Human readability
- Implementation simplicity

## 2.5 Design constraints (Morse, radio, DNF, fragmentation)

### Constraint 1: Morse optimization

#### Limited Morse alphabet

Morse only uses:
- `.` (dot): short signal
- `-` (dash): long signal
- `/` (slash): letter separation
- ` ` (space): word separation

**Shortest letter**: E (.)
**Longest letter**: J (·---)

#### Impact on MML

**Short tag choices**:
- `T:` (Title): - ....
- `H:` (Header): .... .... (shorter than `HEADER:`)
- `P:` (Paragraph): .-- .

**Avoidance of problematic characters**:
- No parentheses: difficult in Morse
- No brackets: possible confusion
- Use of `|` to separate values

### Constraint 2: Radio transmission

#### Voice transmission characteristics

**Limiting factors**:
- Background noise
- Operator accent
- Fatigue
- Stress

**MML solutions**:
- Short phonetic tags
- Clear separation (`:`)
- Pronounceable whole words
- Tolerated repetition

#### Transmission example

**Operator A**: "Tango colon Emergency report"
**Operator B**: "T received"
**Operator A**: "Hotel colon Sector Alpha"
**Operator B**: "H received"

### Constraint 3: DNF integration

#### What is DNF?

DNF (Digital Network Fragment) is a transport protocol that:
- Divides messages into fixed-size fragments
- Transports fragments independently
- Reconstitutes the original message
- Tolerates partial losses

#### MML adaptation to DNF

**Line-based structure**: Each MML line is a natural fragment
```
T:Emergency report
H:Sector Alpha
M:Victims|12
```

**Becomes in DNF**:
- Fragment 1: "T:Emergency report"
- Fragment 2: "H:Sector Alpha"
- Fragment 3: "M:Victims|12"

**Advantage**: Reconstruction possible even if fragments are mixed or lost.

### Constraint 4: Fragmentation resilience

#### Types of fragmentation

1. **Network fragmentation**: Lost IP packets
2. **Physical fragmentation**: Torn pages
3. **Temporal fragmentation**: Interrupted messages
4. **Human fragmentation**: Transmission errors

#### Resilience strategies

**MML design rules**:
- Each line = complete information unit
- No dependency between lines
- Reconstruction possible in any order
- Redundant information tolerated

## 2.6 MML architectural principles

### Principle 1: Line as atomic unit

#### Definition
Each MML line contains complete and independent information.

#### Advantages
- Easy reconstruction after fragmentation
- Line-by-line parsing possible
- Simplified debugging
- Asynchronous transmission

#### Implementation
```javascript
function processMMLLine(line) {
  const [tag, content] = line.split(':', 2);
  return {
    tag: tag.trim(),
    content: content.trim(),
    valid: tag.length > 0 && content.length > 0
  };
}
```

### Principle 2: Clear signal/noise separation

#### Quality metrics

**Optimal signal-to-noise ratio**:
- **Signal**: Useful information (document content)
- **Noise**: Syntax characters (tags, separators)

**MML**: Ratio ~80% (very high signal)
**XML**: Ratio ~50% (many tags)
**JSON**: Ratio ~70% (quotes and structure)

### Principle 3: Controlled extension

#### Philosophy
Add a new tag only if:
1. It covers a universal need
2. It cannot be expressed with existing tags
3. It remains simple and short

#### Extension process
1. **Proposal**: Need description
2. **Validation**: Test in real environments
3. **Implementation**: Addition to standard
4. **Backward compatibility**: No breaking changes

### Principle 4: Design for failure

#### Acceptance of imperfection

**Principle**: An imperfect system that works is preferable to a perfect system that fails.

**MML implementations**:
- **Graceful degradation**: Works partially damaged
- **Best effort parsing**: Ignores invalid lines
- **Recovery mechanisms**: Reconstruction from fragments

### Global architectural vision

MML is not designed as a complex programming language or sophisticated database system. It's a **fundamental communication protocol** that places **reliability** and **accessibility** above **expressive power**.

In a world where complex systems regularly fail, MML offers a simple but essential guarantee: **information will circulate, even in the most difficult conditions**.

---

**Chapter conclusion**: MML foundations are not arbitrary choices but rational responses to real constraints of communication in extreme environments. Each architectural decision serves a specific purpose: maximize resilience while preserving simplicity and universality.

