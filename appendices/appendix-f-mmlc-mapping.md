# Appendix F — MML → MMLC Mapping

## Correspondence tables for MMLC compression

MMLC (MML Compressed) uses a numeric mapping system to reduce the size of MML documents. This appendix defines the official correspondence tables.

### Main tag table

| MML Tag | MMLC Code | Usage Frequency | Morse Gain |
|---------|-----------|-----------------|------------|
| `T:` | `1:` | Very high | 4 → 5 units |
| `H:` | `2:` | Very high | 4 → 5 units |
| `P:` | `4:` | Very high | 2 → 5 units |
| `M:` | `3:` | High | 2 → 5 units |
| `L:` | `5:` | Medium | 2 → 5 units |
| `IMG:` | `6:` | Low | 4 → 5 units |
| `C:` | `7:` | Low | 2 → 5 units |
| `Q:` | `8:` | Low | 2 → 5 units |
| `CFG:` | `9:` | Very low | 4 → 5 units |
| `PKT:` | `10:` | Very low | 4 → 6 units |

### Frequent words table (Integrated dictionary)

| Word/Expression | MMLC Code | Category | Original Length |
|----------------|-----------|----------|-----------------|
| `Report` | `R` | Document | 7 characters |
| `Urgent` | `U` | Priority | 6 characters |
| `Critical` | `C` | Severity | 8 characters |
| `Patient` | `P1` | Medical | 7 characters |
| `Victim` | `V` | Humanitarian | 7 characters |
| `Sector` | `S` | Geographic | 7 characters |
| `Evacuation` | `E` | Action | 10 characters |
| `Medical` | `M` | Domain | 7 characters |
| `Stable` | `S1` | Status | 6 characters |
| `Alert` | `A` | Signal | 6 characters |

### Extended abbreviations table

#### Temporal words
| Word | Code | Word | Code |
|------|------|------|------|
| `Today` | `AJ` | `Tomorrow` | `DM` |
| `Yesterday` | `HI` | `Hour` | `H` |
| `Minute` | `M` | `Second` | `S` |

#### Geographic words
| Word | Code | Word | Code |
|------|------|------|------|
| `North` | `N` | `South` | `S` |
| `East` | `E` | `West` | `O` |
| `Center` | `CE` | `City` | `VI` |
| `Zone` | `Z` | `Sector` | `SE` |

#### Medical words
| Word | Code | Word | Code |
|------|------|------|------|
| `Wounded` | `BL` | `Hospital` | `HO` |
| `Doctor` | `DR` | `Nurse` | `IN` |
| `Ambulance` | `AM` | `Emergency` | `UR` |

## Compression algorithm

### Step 1: Tag mapping
```
Function compress_tag(mml_tag):
    mapping_table = {
        "T": "1", "H": "2", "M": "3", "P": "4",
        "L": "5", "IMG": "6", "C": "7", "Q": "8",
        "CFG": "9", "PKT": "10"
    }
    return mapping_table[mml_tag] + ":"
```

### Step 2: Frequent word substitution
```
Function compress_words(text):
    dictionary = {
        "Report": "R", "Urgent": "U", "Critical": "C",
        "Patient": "P1", "Victim": "V", "Sector": "S",
        "Evacuation": "E", "Medical": "M", "Stable": "S1",
        "Alert": "A"
    }

    For each word in dictionary:
        text = replace(text, word, dictionary[word])

    Return text
```

### Step 3: Complete compression
```
Function compress_mml(mml_text):
    compressed_lines = []

    For each line in mml_text.split('\n'):
        line = line.trim()
        If line contains ':':
            tag, content = line.split(':', 2)
            compressed_tag = compress_tag(tag.trim())
            compressed_content = compress_words(content.trim())
            compressed_lines.append(compressed_tag + compressed_content)

    Return '\n'.join(compressed_lines)
```

## Compression examples

### Example 1: Simple document
**Original MML**:
```
T:Emergency report
H:Current situation
P:Maximum alert activated
M:Severity|Critical
```

**Compressed MMLC**:
```
1:R emergency
2:S current
4:A maximum activated
3:Severity|C
```

**Gain**: 89 characters → 58 characters (35% reduction)

### Example 2: Medical report
**Original MML**:
```
T:Medical report
H:Patient A
M:Status|Stable
M:Diagnosis|Fracture
P:Patient stable under morphine
```

**Compressed MMLC**:
```
1:R medical
2:P1 A
3:Status|S1
3:Diagnosis|Fracture
4:P1 S1 under morphine
```

**Gain**: 102 characters → 71 characters (30% reduction)

### Example 3: Complete emergency transmission
**Original MML**:
```
PKT:EMERGENCY-001
T:Emergency report
H:Sector Alpha
M:Victims|5
P:Urgent evacuation required
Q:Absolute priority
```

**Compressed MMLC**:
```
10:EMERGENCY-001
1:R emergency
2:Z Alpha
3:V|5
4:E U required
8:A priority
```

**Gain**: 125 characters → 76 characters (39% reduction)

## Specialized tables by domain

### Advanced medical dictionary
```json
{
  "Doctor": "MD", "Surgery": "CH", "Resuscitation": "REA",
  "Cardiac": "CARD", "Respiratory": "RESP", "Trauma": "TRAU",
  "Hemorrhage": "HEMO", "Infection": "INF", "Fracture": "FRAC"
}
```

### Military dictionary
```json
{
  "Commander": "CMD", "Soldier": "SOL", "Position": "POS",
  "Enemy": "ENN", "Ally": "ALL", "Reinforcement": "REN",
  "Evacuation": "EVAC", "Perimeter": "PERI", "Security": "SEC"
}
```

### Humanitarian dictionary
```json
{
  "Refugees": "REF", "Aid": "AID", "Food": "NOU",
  "Water": "EAU", "Tent": "TEN", "Medication": "MED",
  "Distribution": "DIST", "Camp": "CAMP", "Volunteer": "VOL"
}
```

## MMLC → MML decompression

### Decompression algorithm
```javascript
function decompressMMLC(mmlcText) {
  const tagMap = {
    '1': 'T', '2': 'H', '3': 'M', '4': 'P', '5': 'L',
    '6': 'IMG', '7': 'C', '8': 'Q', '9': 'CFG', '10': 'PKT'
  };

  const wordMap = {
    'R': 'Report', 'U': 'Urgent', 'C': 'Critical',
    'P1': 'Patient', 'V': 'Victim', 'S': 'Sector',
    'E': 'Evacuation', 'M': 'Medical', 'S1': 'Stable',
    'A': 'Alert'
  };

  return mmlcText.split('\n').map(line => {
    if (!line.includes(':')) return line;

    const [tag, content] = line.split(':', 2);
    const decompressedTag = tagMap[tag] || tag;
    let decompressedContent = content;

    // Decompress words
    Object.entries(wordMap).forEach(([code, word]) => {
      decompressedContent = decompressedContent.replace(
        new RegExp(`\\b${code}\\b`, 'g'), word
      );
    });

    return `${decompressedTag}:${decompressedContent}`;
  }).join('\n');
}
```

## Performance metrics

### Average gains by domain

| Domain | Average Gain | Recommended Usage |
|--------|--------------|-------------------|
| General | 25-35% | All documents |
| Medical | 30-40% | Medical reports |
| Emergency | 35-45% | Vital communications |
| Technical | 20-30% | Documentation |

### Impact on Morse transmission

#### Transmission time (example)
- **Original MML**: "T:Report H:Situation P:Alert" → ~45 seconds
- **MMLC**: "1:R 2:S 4:A" → ~25 seconds
- **Gain**: 44% reduced transmission time

### Compression limits
- **Limited dictionary**: Uncovered words remain unchanged
- **No binary compression**: Text format preserved
- **Domain-specific**: Best gain with adapted dictionaries

## Future extensions

### MMLC 2.0: Adaptive compression
- **Machine learning**: Automatically generated dictionary
- **Contextual compression**: Adaptation to document domain
- **Variable codes**: Optimized code lengths

### AI integration
- **Dictionary generation**: Automatic corpus analysis
- **Predictive compression**: Anticipating next words
- **Real-time optimization**: Adaptation during transmission

---

**MMLC mapping tables enable efficient compression while preserving human readability and MML format resilience.**

