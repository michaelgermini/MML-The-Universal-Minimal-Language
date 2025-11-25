# Chapter 3 — MML Syntax

## 3.1 General document structure

### Anatomy of an MML document

An MML document is composed of **text lines** organized according to an implicit hierarchical structure.

#### Basic structure
```
T:Main document title
H:First level section
P:Content paragraph
H:Subsection
P:Subsection content
```

#### Conceptual HTML equivalent
```html
<h1>Main document title</h1>
<h2>First level section</h2>
<p>Content paragraph</p>
<h2>Subsection</h2>
<p>Subsection content</p>
```

### Fundamental structure rules

#### Rule 1: One element per line
Each line contains only one MML element.

**Valid**:
```
T:My title
P:My paragraph
```

**Invalid**:
```
T:My title P:My paragraph
```

#### Rule 2: Hierarchy by position
Hierarchy is determined by order of appearance, not by nesting.

**Implicit structure**:
```
T:Main document
H:Chapter 1
P:Chapter 1 content
H:Chapter 2
P:Chapter 2 content
```

#### Rule 3: UTF-8 encoding
All MML documents use UTF-8 encoding.

## 3.2 Basic rules (TAG:, ;, line breaks)

### Elementary syntax

#### Canonical format
```
TAG:content
```

**Components**:
- **TAG**: Short tag (1-4 characters)
- **:** : Mandatory separator (colon)
- **content**: Free text (can be empty)

### Line break handling

#### Line break rule
- Each element occupies exactly one line
- Empty lines are ignored
- Document ends with optional line break

#### Valid example
```
T:My document
P:Line 1
P:Line 2

P:Line 4
```

### Space handling

#### Significant spaces
- **Before TAG**: Ignored (indentation allowed)
- **After :**: Ignored
- **In content**: Preserved

#### Examples
```
  T:  My title
P:   Content with spaces
```

**Equivalent to**:
```
T:My title
P:Content with spaces
```

## 3.3 Minimal character set (designed for Morse)

### Morse alphabet and constraints

#### Basic Morse characters
- **Letters**: A-Z (uppercase only)
- **Digits**: 0-9
- **Limited punctuation**: . , : ; - / | ?

#### Excluded characters
- **Parentheses**: ( ) [ ] { } → confusion in oral transmission
- **Complex symbols**: @ # $ % & * → difficult in Morse
- **Accents**: é è à → complex transmission

### Content adaptation

#### Encoding strategy
For non-Morse characters, use equivalents:
- **é** → **e**
- **à** → **a**
- **ç** → **c**
- **œ** → **oe**

#### Adaptation example
**Original text**: "État d'urgence à Paris"
**MML version**: "Etat d urgence a Paris"

## 3.4 Fundamental tags

### T: — Title

#### Usage
Defines the main document title.

#### Syntax
```
T:Title text
```

#### Examples
```
T:Emergency report
T:Survival guide
T:Medical inventory
```

#### Rules
- One T: per document (recommended)
- Place at document start
- Short and descriptive text

### H: — Section / header

#### Usage
Defines document sections and subsections.

#### Syntax
```
H:Section text
```

#### Examples
```
H:Introduction
H:Required materials
H:Emergency procedure
H:Chapter 1: The basics
```

#### Implicit hierarchy
Hierarchy is determined by order of appearance:
```
T:Survival manual
H:Chapter 1
H:Section 1.1
H:Section 1.2
H:Chapter 2
```

### P: — Paragraph

#### Usage
Contains the main document text.

#### Syntax
```
P:Paragraph text
```

#### Examples
```
P:This is a normal text paragraph.
P:In case of emergency, follow this procedure.
P:The patient presents the following symptoms.
```

#### Rules
- Text can be multiline (but remains on one physical line)
- No length limitation
- Supports spaces and punctuation

### L: — Link

#### Usage
Creates references to other resources.

#### Syntax
```
L:Link text|URL or reference
```

#### Examples
```
L:View map|https://maps.example.com/sector-alpha
L:Complete manual|survival-manual.pdf
L:GPS coordinates|48.8566,2.3522
L:Radio contact|Frequency 145.500 MHz
```

#### Supported formats
- **Web URLs**: http://, https://
- **Local files**: .pdf, .doc, etc.
- **Coordinates**: latitude,longitude
- **Radio frequencies**: MHz, kHz
- **Internal references**: other MML documents

### IMG: — Image

#### Usage
References images or diagrams.

#### Syntax
```
IMG:Image description|URL or path
```

#### Examples
```
IMG:Sector map|sector-map.png
IMG:Electrical diagram|wiring-diagram.jpg
IMG:Procedure flowchart|flowchart-process.svg
```

#### Rules
- Description mandatory (accessibility)
- Support for common formats: PNG, JPG, SVG, GIF

### C: — Code block

#### Usage
Presents computer code or technical commands.

#### Syntax
```
C:Code content
```

#### Examples
```
C:SELECT * FROM patients WHERE status = 'critical'
C:function backupData() { return database.save(); }
C:scp file.txt user@server:/backup/
```

#### Rules
- Exact preservation of spaces and special characters
- No escaping necessary (except for :)

### Q: — Quote

#### Usage
Marks quoted text or important information.

#### Syntax
```
Q:Quote text
```

#### Examples
```
Q:In case of emergency, stay calm and follow established procedures.
Q:According to Dr. Smith: "Vaccination saves lives"
Q:Standard protocol: isolate, identify, treat
```

#### Special usage
Can be used for alert messages or standard protocols.

### M: — Metadata

#### Usage
Adds structured information about the document or its elements.

#### Syntax
```
M:Key|Value
```

#### Examples
```
M:Author|Dr. Marie Dubois
M:Date|2025-11-15
M:Version|1.2
M:Priority|CRITICAL
M:ID|PATIENT-001
```

#### Rules
- Key-value format with | separator
- Keys in uppercase (convention)
- Values can contain spaces

### CFG: — Configuration

#### Usage
Defines system configuration parameters.

#### Syntax
```
CFG:Configuration section
M:Parameter|Value
M:Parameter2|Value2
```

#### Examples
```
CFG:Network settings
M:IP|192.168.1.100
M:Mask|255.255.255.0
M:DNS|8.8.8.8

CFG:Radio settings
M:Frequency|145.500
M:Mode|FM
M:Power|5W
```

### PKT: — DNF container

#### Usage
Defines a container for DNF packets.

#### Syntax
```
PKT:Packet identifier
[packet content]
```

#### Examples
```
PKT:EMERGENCY-MEDICAL
T:Emergency medical report
H:Patient A
M:Status|Critical
P:Immediate evacuation required
```

## 3.5 Multi-content lines

### Complex content handling

#### Problem
Some content requires multiple values or options.

#### MML solution
Use the `|` separator for multiple values.

#### Examples
```
M:Coordinates|48.8566|2.3522|Paris
L:Options|Option A|Option B|Option C
CFG:Backup|daily|7 days|compressed
```

### Multiple value parsing

#### Example function
```javascript
function parseMultiValue(content) {
  return content.split('|').map(item => item.trim());
}

// Example
parseMultiValue("48.8566|2.3522|Paris")
// Result: ["48.8566", "2.3522", "Paris"]
```

## 3.6 Escaping and avoiding ambiguities

### Special characters in content

#### Problem
The `:` character can appear in normal content.

#### Solutions
1. **Context**: The first `:` in a line always separates TAG from content
2. **Escaping**: Double the `:` to use it in content

#### Examples
```
P:Time: 14:30 (simple)
P:JSON format: {"key": "value"} (requires attention)
```

#### Escaping rule
If content must start with a valid TAG, use escaping:
```
P:Here is an example: T:Title in text
```

### Vertical bar handling

#### In metadata
The `|` bar separates components of M:.

#### Escaping
To use `|` in content:
```
M:Description|Text with \| special character
```

## 3.7 Simple and advanced examples

### Simple example: Postcard

```
T:Postcard
M:From|Marie
M:To|Pierre
M:Location|Paris
P:Dear Pierre,
P:I hope you are well. Paris is beautiful this season.
P:Best regards,
P:Marie
```

### Intermediate example: Incident report

```
T:Incident report
M:ID|INC-2025-001
M:Date|2025-11-15
M:Location|Sector Alpha
M:Severity|CRITICAL

H:Circumstances
P:At 14:30, explosion in storage sector.
P:Probable cause: gas leak.

H:Consequences
P:2 minor injuries
P:Damaged equipment: rack A3-A5
P:Sector evacuation in progress

H:Actions taken
P:Medical team deployed
P:Zone secured
P:Investigation in progress

L:Incident photos|incident-photos.zip
L:Detailed technical report|complete-report.pdf
```

### Advanced example: Complete system configuration

```
T:Backup server configuration
M:Version|2.1
M:Last modified|2025-11-15T10:30:00Z

CFG:Main network
M:Interface|eth0
M:IP|192.168.1.100
M:Mask|255.255.255.0
M:Gateway|192.168.1.1

CFG:Backup network
M:Interface|eth1
M:IP|10.0.0.100
M:Mask|255.255.255.0
M:Gateway|10.0.0.1

CFG:Critical services
M:Web|active
M:Database|active
M:Monitoring|active

CFG:Security settings
M:Firewall|active
M:SSH|key only
M:Updates|automatic

H:Failover procedures
P:In case of main network failure:
P:1. Check eth1 connectivity
P:2. Execute failover.sh script
P:3. Notify technical team
P:4. Test critical services

C:# Automatic failover script
C:if ! ping -c 1 192.168.1.1; then
C:    systemctl restart network
C:    /usr/local/bin/failover.sh
C:fi
```

### Specialized example: Emergency transmission

```
PKT:EMERGENCY-001
T:PRIORITY MEDICAL ALERT
M:Priority|URGENT
M:Sender|Dr. Sarah Chen
M:Location|Delta Camp - Sector 7
M:Timestamp|2025-11-15T14:45:00Z

H:Victim 1 - 8 year old child
M:Status|Critical
M:Symptoms|Drop in blood pressure, convulsions
M:Diagnosis|Severe dehydration
M:Treatment|Immediate IV, oxygen
P:Heliport evacuation required within the hour

H:Victim 2 - 35 year old adult
M:Status|Stable
M:Symptoms|Open fracture right leg
M:Treatment|Immobilization, antibiotics
P:Can wait for ground transport

H:Available resources
P:2 doctors, 1 nurse
P:Basic medications available
P:Oxygen: 3 bottles remaining
P:Electricity: generator operational

Q:URGENT: Need medical reinforcements and air evacuation
L:GPS coordinates|14.7566|-17.4567
L:Radio frequency|148.500 MHz
```

---

**Chapter conclusion**: MML syntax is deliberately simple and restrictive. This approach may seem constraining at first glance, but it guarantees the **reliability**, **readability** and **universal transmissibility** of the format. Each rule serves a specific purpose in the constrained communication ecosystem where MML is intended to operate.

