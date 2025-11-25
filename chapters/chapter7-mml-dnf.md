# Chapter 7 — MML + DNF

## 7.1 The direct link between the two technologies

### Birth of an ecosystem

MML and DNF (Digital Network Fragment) are designed as **two sides of the same coin**: resilient communication.

#### Common origin
- **MML**: Minimal serialization format
- **DNF**: Fragmented transport protocol
- **Together**: Post-collapse communication system

#### Shared philosophy
- **Resilience**: Work despite damage
- **Simplicity**: Accessible to all
- **Universality**: Independent of infrastructures

## 7.2 Why DNF is the ideal transport for MML

### Technical affinities

#### Complementary structure
- **MML**: Content organized into autonomous lines
- **DNF**: Transport of independent fragments

#### Synergistic advantages
- **Natural fragmentation**: Each MML line = DNF fragment
- **Automatic reconstitution**: MML DOM reconstructed from fragments
- **Loss tolerance**: Partially reconstructable documents

### Perfect integration example

**MML document**:
```
T:Emergency report
H:Sector A
M:Victims|5
P:Stable status
H:Sector B
M:Victims|3
P:Reinforcements required
```

**Generated DNF fragments**:
- **Fragment 1**: `T:Emergency report`
- **Fragment 2**: `H:Sector A`
- **Fragment 3**: `M:Victims|5`
- **Fragment 4**: `P:Stable status`
- **Fragment 5**: `H:Sector B`
- **Fragment 6**: `M:Victims|3`
- **Fragment 7**: `P:Reinforcements required`

## 7.3 PKT fragmentation

### Packet structure

#### MML PKT format
```
PKT:UNIQUE-IDENTIFIER
[fragmented MML content]
END
```

#### Concrete example
```
PKT:MED-URG-001
T:Emergency medical report
H:Patient A - 45 years
M:Diagnosis|Heart attack
P:Severe chest pain
END
```

### Transport metadata management

#### DNF headers
- **ID**: Unique packet identifier
- **TTL**: Time To Live
- **Priority**: Priority level
- **Source**: Message origin

#### Integration with MML
```
PKT:SEC-ALERT-001:TTL=3600:PRIORITY=HIGH
T:Maximum security alert
H:Intrusion detected
M:Zone|East Wing
P:Immediate evacuation
END
```

## 7.4 Automatic reconstitution

### Reconstruction algorithm

#### Three-step process

1. **Collection**: Gather all fragments with same ID
2. **Validation**: Verify integrity of each fragment
3. **Assembly**: Reconstruct document according to DOM logic

#### Simplified implementation
```javascript
class MMLDNFReconstructor {
  constructor() {
    this.fragments = new Map(); // ID -> fragments[]
  }

  addFragment(packetId, fragment) {
    if (!this.fragments.has(packetId)) {
      this.fragments.set(packetId, []);
    }
    this.fragments.get(packetId).push(fragment);
  }

  reconstruct(packetId) {
    const fragments = this.fragments.get(packetId) || [];
    if (fragments.length === 0) return null;

    // Sort by logical priority
    const sortedFragments = this.sortFragments(fragments);

    // Build document
    const document = this.buildDocument(sortedFragments);

    return document;
  }

  sortFragments(fragments) {
    const priority = { 'T': 1, 'H': 2, 'M': 3, 'P': 4 };
    return fragments.sort((a, b) => {
      const aType = a.split(':')[0];
      const bType = b.split(':')[0];
      return (priority[aType] || 99) - (priority[bType] || 99);
    });
  }

  buildDocument(fragments) {
    const parser = new MMLParser();
    const mmlText = fragments.join('\n');
    return parser.parse(mmlText);
  }
}
```

### Partial loss handling

#### Degraded reconstitution
If some fragments are lost, the document remains partially usable.

**Example**:
- **Losses**: Detailed metadata
- **Preserved**: Main structure and essential content

## 7.5 Use cases: antennas, Bluetooth, mesh, human → human

### Radio antenna transmission

#### Typical scenario
- **Transmitter**: Field operator with portable radio
- **Transport**: VHF/UHF radio wave
- **Receiver**: Coordination center

#### Advantages
- **Range**: Several kilometers
- **Speed**: Real-time
- **Reliability**: Good in coverage area

### Bluetooth mesh networks

#### IoT applications
- **Environmental sensors** transmitting MML data
- **Self-organizing rescue networks**
- **Communication between drones** and operators

#### Example
```
PKT:SENSOR-001
T:Sensor data
M:Type|Temperature
M:Value|23.5°C
M:Battery|85%
END
```

### Human mesh networks

#### Peer-to-peer communication
- **Transport**: Courier on foot/bike
- **Storage**: Memorization or paper
- **Resumption**: Transmission to next station

#### Ultimate robustness
- **No energy** required (in theory)
- **Works** even without technology
- **Redundancy** by multiplying couriers

### Human → machine → human chain

#### Complete process
1. **Human observation** → **MML encoding**
2. **Transmission** by any means → **Temporary storage**
3. **Reception** by automated system → **Algorithmic processing**
4. **Conversion** to useful format → **Distribution** to recipients
5. **Final human decoding**

#### Concrete example: Tsunami alert
1. **Witness**: "5m wave approaching!"
2. **Encodes**: `T:TSUNAMI ALERT H:5M WAVE P:EVACUATE IMMEDIATELY`
3. **Transmits**: By radio or courier
4. **System receives**: Converts to automatic alert
5. **Population alerted**: By sirens and SMS

## 7.6 Validation, authenticity, hashing

### Security in MML+DNF ecosystem

#### Hashing for integrity
Each packet can include its cryptographic hash.

#### Secure format
```
PKT:SECURE-MSG-001
T:Authenticated document
[MML content]
HASH:SHA256:abcdef123456...
SIGN:ED25519:key_id:signature...
END
```

### Source authentication

#### Lightweight public keys
- **ED25519**: Fast and compact signature
- **Embedded keys**: In MML devices
- **Trust chain**: Validation by recognized authorities

### Content validation

#### Automatic rules
- **Consistency**: Metadata verification
- **Valid range**: Temperatures between -100°C and +100°C
- **Format**: GPS coordinate validation

#### Validation example
```javascript
class MMLValidator {
  validate(document) {
    const errors = [];

    // Check title presence
    if (!document.title) {
      errors.push("Missing title");
    }

    // Validate GPS coordinates
    if (document.metadata.coordinates) {
      if (!this.isValidGPS(document.metadata.coordinates)) {
        errors.push("Invalid GPS coordinates");
      }
    }

    // Check value ranges
    if (document.metadata.temperature) {
      const temp = parseFloat(document.metadata.temperature);
      if (temp < -100 || temp > 100) {
        errors.push("Temperature out of valid range");
      }
    }

    return errors;
  }

  isValidGPS(coord) {
    const gpsRegex = /^-?\d{1,3}\.\d+,-?\d{1,3}\.\d+$/;
    return gpsRegex.test(coord);
  }
}
```

---

**Chapter conclusion**: The MML+DNF integration represents the culmination of minimalist philosophy. Together, they create a communication system that can survive the collapse of modern infrastructures. This synergy transforms two simple technologies into a resilient communication infrastructure capable of functioning in the most extreme conditions, proving that well-designed simplicity can surpass sophisticated complexity.

