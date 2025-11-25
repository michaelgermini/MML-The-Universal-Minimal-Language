# Chapter 5 — MMLC: Compressed Version

## 5.1 Why compress?

### Compression need

Despite its conciseness, MML remains a text format that can be optimized for very constrained transmissions.

#### Scenarios requiring compression

1. **Slow Morse transmission**: Every character counts
2. **Low bandwidth channels**: Satellite, packet radio
3. **Limited storage**: Microcontrollers, memory cards
4. **Transmission cost**: Paid radio, satellite

#### MMLC advantages

- **40-60% reduction** in message size
- **Acceleration** of Morse transmissions
- **Savings** in bandwidth and energy
- **Compatibility** with existing equipment

### Size comparison

#### Example: Weather bulletin

**Original MML** (124 characters):
```
T:Weather bulletin
H:Forecast
M:Temperature|22°C
M:Wind|15 km/h
P:Sunny weather
```

**Compressed MMLC** (68 characters):
```
1:Weather
2:Forecast
3:Temperature|22°C
3:Wind|15 km/h
4:Sunny weather
```

**Gain**: 45% size reduction.

## 5.2 Tag mapping → digits (1, 2, 3…)

### Standard correspondence table

#### Fundamental tags
- **1** = T: (Title)
- **2** = H: (Header/Section)
- **3** = M: (Metadata)
- **4** = P: (Paragraph)
- **5** = L: (Link)
- **6** = IMG: (Image)
- **7** = C: (Code)
- **8** = Q: (Quote)
- **9** = CFG: (Configuration)
- **10** = PKT: (Packet)

#### Possible extension
- **11-99**: Custom tags by domain
- **100+**: Composite macros

### MMLC syntax

#### Compressed format
```
number:content
```

#### Examples
```
1:Emergency report
2:Sector Alpha
3:Victims|12
4:Evacuation required
```

**MML equivalent**:
```
T:Emergency report
H:Sector Alpha
M:Victims|12
P:Evacuation required
```

## 5.3 Advantages for Morse

### Morse optimization

#### Short characters in Morse
Digits are shorter than letters:
- **1**: ·---- (5 signs)
- **T**: - (4 signs)

But especially: **fewer characters total**.

#### Transmission example

**MML**: "T:Report H:Sector M:Status|OK"
- Letters: T H S E C T E U R M S T A T U T O K
- Estimated duration: ~2 minutes

**MMLC**: "1:Report 2:Sector 3:Status|OK"
- Codes: 1 : R A P P O R T  2 : S E C T E U R  3 : S T A T U T | O K
- Estimated duration: ~1.3 minute

**Gain**: 35% transmission time.

### Improved resilience

#### Fewer characters = fewer errors
- **Reduced error rate** proportional to number of characters
- **Easier correction** for operators
- **Simplified recovery** in case of interruption

## 5.4 Short code tables

### Integrated dictionary

#### Compressed frequent words
In addition to tags, MMLC can compress common words:

- **A** = Alert
- **B** = Bulletin
- **C** = Critical
- **D** = Danger
- **E** = Evacuation
- **F** = Fire
- **G** = Gravity
- **H** = Hospital
- **I** = Important
- **J** = Day
- **K** = km/h (kilometer per hour)
- **L** = Location
- **M** = Medical
- **N** = Normal
- **O** = OK
- **P** = Patient
- **Q** = Quantity
- **R** = Report
- **S** = Stable
- **T** = Temperature
- **U** = Urgent
- **V** = Victim
- **W** = Wind
- **X** = Explosion
- **Y** = °C (degrees Celsius)
- **Z** = Zone

### Dictionary usage

#### Syntax with dictionary
```
1:R
2:Z Alpha
3:V|5
4:E immediate
```

**Expansion**:
```
1:Report
2:Zone Alpha
3:Victim|5
4:Evacuation immediate
```

### Specialized tables

#### Medical dictionary
- **MED1** = Aspirin
- **MED2** = Paracetamol
- **MED3** = Antibiotics
- **DIA1** = Pneumonia
- **DIA2** = Fracture
- **DIA3** = Dehydration

#### Technical dictionary
- **TEC1** = Short circuit
- **TEC2** = Power failure
- **TEC3** = Gas leak

## 5.5 Contextual compression (tokens, dictionary)

### Adaptive compression

#### Frequency analysis
The compressor can analyze a corpus of documents to identify the most frequent words.

#### Analysis example
Out of 100 emergency reports:
- "Patient" appears 85 times
- "Stable" appears 67 times
- "Evacuation" appears 43 times
- "Critical" appears 38 times

#### Generated dictionary
- **P1** = Patient
- **S1** = Stable
- **E1** = Evacuation
- **C1** = Critical

### Domain compression

#### Specialized MMLC
Different compression tables according to context:

- **MMLC-MED**: Medical dictionary
- **MMLC-TECH**: Technical dictionary
- **MMLC-MIL**: Military dictionary
- **MMLC-HUM**: Humanitarian dictionary

#### MMLC-MED example
```
1:R emergency
2:P1 A
3:Status|C1
3:DIA1
4:O2 and MED2 required
```

**Expansion**:
```
T:Emergency report
H:Patient A
M:Status|Critical
M:Diagnosis|Pneumonia
P:Oxygen and Paracetamol required
```

## 5.6 Example HTML → MML → MMLC

### Complete conversion process

#### Original HTML document
```html
<!DOCTYPE html>
<html>
<head>
  <title>Emergency report</title>
  <meta name="author" content="Dr. Smith">
  <meta name="date" content="2025-11-15">
</head>
<body>
  <h1>Current situation</h1>
  <p>Sector Alpha reports 3 injured.</p>
  <p>Status: 2 stable, 1 critical.</p>

  <h2>Required resources</h2>
  <p>Additional medical team required.</p>
  <p>Priority air evacuation.</p>

  <blockquote>
    Remember: speed saves lives.
  </blockquote>
</body>
</html>
```

#### Conversion to MML
```
T:Emergency report
M:Author|Dr. Smith
M:Date|2025-11-15
H:Current situation
P:Sector Alpha reports 3 injured.
P:Status: 2 stable, 1 critical.
H:Required resources
P:Additional medical team required.
P:Priority air evacuation.
Q:Remember: speed saves lives.
```

**Size**: 312 characters

#### Conversion to standard MMLC
```
1:Emergency report
3:Author|Dr. Smith
3:Date|2025-11-15
2:Current situation
4:Sector Alpha reports 3 injured.
4:Status: 2 stable, 1 critical.
2:Required resources
4:Additional medical team required.
4:Priority air evacuation.
8:Remember: speed saves lives.
```

**Size**: 268 characters (14% reduction)

#### Conversion to MMLC with dictionary
```
1:R emergency
3:Author|Dr. Smith
3:J|2025-11-15
2:S current
4:Z Alpha reports 3 V
4:Status: 2 S1, 1 C1
2:R required
4:E med supp req
4:E air U
8:Speed saves lives
```

**Size**: 142 characters (55% reduction)

### Gain analysis

| Format | Characters | Gain | Usage |
|--------|------------|------|-------|
| HTML   | 587       | -    | Web   |
| MML    | 312       | 47%  | Standard |
| MMLC   | 268       | 54%  | Transmission |
| MMLC+Dict | 142    | 76%  | Emergency |

### Compressor implementation

#### JavaScript compression function
```javascript
class MMLCompressor {
  constructor() {
    this.tagMap = {
      'T': '1', 'H': '2', 'M': '3', 'P': '4', 'L': '5',
      'IMG': '6', 'C': '7', 'Q': '8', 'CFG': '9', 'PKT': '10'
    };

    this.wordMap = {
      'Report': 'R', 'Patient': 'P1', 'Stable': 'S1',
      'Evacuation': 'E1', 'Critical': 'C1', 'Urgent': 'U',
      'Zone': 'Z', 'Victim': 'V', 'Medical': 'M'
    };
  }

  compress(mmlText) {
    const lines = mmlText.split('\n');
    const compressedLines = lines.map(line => this.compressLine(line));
    return compressedLines.join('\n');
  }

  compressLine(line) {
    if (!line.includes(':')) return line;

    const [tag, content] = line.split(':', 2);
    const compressedTag = this.tagMap[tag.trim()] || tag;
    const compressedContent = this.compressContent(content);

    return `${compressedTag}:${compressedContent}`;
  }

  compressContent(content) {
    let compressed = content;
    for (const [word, code] of Object.entries(this.wordMap)) {
      compressed = compressed.replace(new RegExp(word, 'g'), code);
    }
    return compressed;
  }

  decompress(mmlcText) {
    // Inverse function for decompression
    const lines = mmlcText.split('\n');
    const decompressedLines = lines.map(line => this.decompressLine(line));
    return decompressedLines.join('\n');
  }

  decompressLine(line) {
    if (!line.includes(':')) return line;

    const [tag, content] = line.split(':', 2);
    const decompressedTag = this.reverseTagMap[tag] || tag;
    const decompressedContent = this.decompressContent(content);

    return `${decompressedTag}:${decompressedContent}`;
  }

  decompressContent(content) {
    let decompressed = content;
    for (const [code, word] of Object.entries(this.reverseWordMap)) {
      decompressed = decompressed.replace(new RegExp(code, 'g'), word);
    }
    return decompressed;
  }
}
```

---

**Chapter conclusion**: MMLC demonstrates that MML can be further optimized for the most constrained environments without losing its human readability. Compression by numeric mapping and contextual dictionary offers significant gains in size and transmission speed, particularly crucial for emergency communications. MMLC proves that minimalism and efficiency can coexist perfectly.

