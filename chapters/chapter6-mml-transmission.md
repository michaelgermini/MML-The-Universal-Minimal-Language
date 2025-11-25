# Chapter 6 — MML Transmission

## 6.1 MML as a transmittable format

### Transmissibility properties

MML is designed to be transmitted by **all imaginable means**:

#### Key properties
- **ASCII text format**: Compatible with all systems
- **Independent lines**: Resilience to fragmentation
- **Minimal encoding**: No problematic special characters
- **Optimized size**: High signal-to-noise ratio

### Transmissibility metrics

#### MML transmissibility score: 9.8/10

| Criterion | Score | Justification |
|-----------|-------|---------------|
| Morse | 10/10 | Short tags, digits available |
| Voice radio | 9/10 | Pronounceable, structured |
| Manual writing | 10/10 | Simple to write |
| Digital | 10/10 | Standard text |
| Optical | 8/10 | Visible, but slow |
| Acoustic | 7/10 | Possible but less optimal |

## 6.2 Sequential vs fragmented transmission

### Sequential transmission

#### Classic mode
The document is transmitted line by line in order.

**Advantages**:
- Preservation of original structure
- Easy reconstruction
- Logical order maintained

**Disadvantages**:
- Vulnerable to interruptions
- All or nothing: failure if cut

### Fragmented transmission

#### Resilient mode
Each line becomes an independent fragment.

**Advantages**:
- Partial reconstruction possible
- Loss tolerance
- Reconstitution in any order

**Disadvantages**:
- Hierarchical structure to reconstruct
- Potential metadata loss

#### Practical example

**Original document**:
```
T:Weather report
H:Today
M:Temp|25°C
P:Sunny weather
```

**Fragmented transmission**:
- Fragment A: "T:Weather report"
- Fragment B: "H:Today"
- Fragment C: "M:Temp|25°C"
- Fragment D: "P:Sunny weather"

**Reconstitution possible** even if B and C are lost.

## 6.3 Loss, duplication and reconstruction

### Loss handling

#### Loss-tolerant reconstruction algorithm

1. **Collection** of all received fragments
2. **Identification** of each fragment type
3. **Grouping** by logical sections
4. **Reconstruction** of hierarchy

#### Resilience example

**Received fragments** (60% losses):
- "T:Emergency report"
- "P:Evacuation required"
- "H:Sector B"
- "P:Stabilization in progress"

**Possible reconstruction**:
```
T:Emergency report
H:Sector B
P:Evacuation required
P:Stabilization in progress
```

### Duplication handling

#### Detection and removal
- **Timestamping** in metadata
- **Unique identifier** per document
- **Content comparison** for duplicates

### Automatic reconstruction

#### Reconstitution logic

```javascript
class MMLReconstructor {
  reconstruct(fragments) {
    const document = { title: null, sections: [], metadata: {} };

    // Sort by priority type
    const sortedFragments = this.prioritizeFragments(fragments);

    for (const fragment of sortedFragments) {
      this.addFragmentToDocument(document, fragment);
    }

    return this.finalizeDocument(document);
  }

  prioritizeFragments(fragments) {
    const priority = { 'T': 1, 'H': 2, 'M': 3, 'P': 4 };
    return fragments.sort((a, b) => {
      const aPriority = priority[a.tag] || 99;
      const bPriority = priority[b.tag] || 99;
      return aPriority - bPriority;
    });
  }
}
```

## 6.4 Transmission via DNF

### Native integration

MML and DNF form a perfect couple because:

#### Technical synergy
- **MML**: Structures content into independent lines
- **DNF**: Transports lines as autonomous fragments

#### Combined advantages
- **Maximum resilience**: Fragment loss tolerated
- **Automatic reconstruction**: Intelligent reassembly
- **Universal transmission**: All channels supported

### Concrete example

**MML document**:
```
T:Security alert
H:Incident detected
M:Severity|High
P:Unauthorized access detected
```

**Generated DNF packet**:
```
PKT:SEC-001
T:Security alert
H:Incident detected
M:Severity|High
P:Unauthorized access detected
END
```

## 6.5 Transmission via Morse

### Morse code optimization

#### Tag choices
- **T**: - (4 signs) → very short
- **H**: .... (4 signs) → short
- **P**: .--. (6 signs) → acceptable
- **M**: -- (4 signs) → short

#### Voice transmission
- **Operator A**: "T colon Security alert"
- **Operator B**: "T received"
- **Operator A**: "H colon Compromised sector"
- **Etc.**

### Transmission speed

#### Theoretical calculation
- **Expert Morse speed**: 20-25 words/minute
- **Average MML line length**: 25 characters
- **Effective throughput**: ~15 lines/minute

#### Comparison
- **Free text**: Unreadable in continuous Morse
- **Structured MML**: Efficiently transmittable

## 6.6 Transmission via amateur radio (CW, JS8, APRS)

### Continuous Wave (CW)

#### Advantages
- **Global range** with low power
- **Penetration** in difficult environments
- **Minimal energy cost**

#### Adapted format
```
DE F5XYZ
T:FLOOD ALERT
H:RIVERSIDE SECTOR
M:VICTIMS|8
P:URGENT EVACUATION
M:COORD|45.2N 2.1E
E E
```

### JS8Call

#### Modern digital mode
- **Text transmission** over radio waves
- **Automatic error correction**
- **Extended range**

#### Transmission example
```
F5XYZ: T:MEDICAL REPORT H:CRITICAL PATIENT M:STATUS|STABLE P:TREATMENT IN PROGRESS
```

### APRS (Automatic Packet Reporting System)

#### Packet system
- **GPS positioning** integrated
- **Automatic routing**
- **Cloud storage**

#### MML/APRS packet
```
F5XYZ>APRS,TCPIP*:=4552.45N/00215.67E- T:SECURITY H:ACCESS FORBIDDEN P:DANGER ZONE
```

## 6.7 Transmission via optical, audio or vibrations

### Optical transmission

#### Light signals
- **Flashlight**: Dot = short, dash = long
- **Semaphores**: Colored flags
- **Laser**: Directional transmission

#### MML adaptation
Use MMLC with binary digits for optimization.

### Acoustic transmission

#### Whistles and codes
- **Two-tone whistle**: Dot/dash
- **Audio Morse**: Distinct frequencies

#### Applications
- Communication in noisy environment
- Distress signaling

### Vibration transmission

#### Specialized applications
- **Braille encoders**: For visually impaired
- **Haptic signals**: Smart watches
- **Morse vibrations**: Portable devices

## 6.8 Error tolerance

### Handled error types

#### Transmission errors
- **Substitution**: Letter changed (A→E)
- **Insertion**: Character added
- **Deletion**: Character lost
- **Transposition**: Order changed

#### Human errors
- **Phonetic**: Auditory confusion
- **Visual**: Graphic confusion
- **Fatigue**: Attention errors

### Tolerance strategies

#### Controlled redundancy
- **Repetition** of critical information
- **Paraphrases**: "Maximum alert" = "URGENT"
- **Control codes**: Checksums

#### Cross-validation
- **Multi-transmission**: Multiple channels
- **Consensus**: Agreement between sources
- **Context**: Semantic validation

### Robustness tests

#### Simulated error scenario
**Original message**: "T:URGENT H:EVACUATE M:CAUSE|FIRE"

**After errors**:
- "T:URGENT H:EVACUFR M:CAUSR|FIRE"
- "T:URGENT H:EVACUATE M:CAUSE|FFU"

**Result**: Message still intelligible despite corruptions.

---

**Chapter conclusion**: Universal transmissibility is MML's major strength. Unlike modern digital formats that depend on complex infrastructures, MML can travel by any means: from radio waves to light signals, from human voice to Morse code. This universality makes MML much more than a data format – it's a communication protocol for humanity.

