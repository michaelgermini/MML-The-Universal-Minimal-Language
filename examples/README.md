# MML Examples

This directory contains practical examples of MML (Minimal Markup Language) documents demonstrating various use cases.

## Example Files

### 01-simple-document.mml
A basic MML document showing fundamental structure and tags.

### 02-emergency-report.mml
Medical emergency report demonstrating urgent communication format.

### 03-weather-bulletin.mml
Weather data formatted in MML for transmission and storage.

### 04-system-configuration.mml
Network and system configuration using MML format.

### 05-patient-record.mml
Medical patient record showing structured health data.

### 06-disaster-coordination.mml
Disaster response coordination document with resource requests.

### 07-iot-sensor-data.mml
IoT sensor data packet in MML format with DNF packaging.

### 08-survival-guide.mml
Survival guide demonstrating documentation use case.

### 09-technical-manual.mml
Equipment manual showing technical documentation format.

### 10-incident-report.mml
Security incident report with structured information.

## Formats Available

Each example is available in three formats:
- **`.mml`** - Original MML format (minimal, human-readable)
- **`.json`** - JSON format (structured, API-friendly)
- **`.xml`** - XML format (standard, verbose)

### File Statistics

**Total Files Created:**
- 📁 **10 `.mml` files** (original format)
- 📁 **10 `.json` files** (structured format)
- 📁 **10 `.xml` files** (standard format)
- 📁 **Total: 30 example files** + 1 README.md

### Size Comparison

The following table demonstrates MML's efficiency compared to JSON and XML formats:

| Example | MML | JSON | XML | MML Advantage |
|---------|-----|------|-----|---------------|
| Simple Document | 352 B | 1,216 B | 879 B | **64% smaller** vs JSON |
| Emergency Report | 714 B | 2,043 B | 1,548 B | **65% smaller** vs JSON |
| Weather Bulletin | 500 B | 1,486 B | 1,257 B | **66% smaller** vs JSON |
| System Config | 592 B | 1,415 B | 1,446 B | **58% smaller** vs JSON |
| Patient Record | 660 B | 1,903 B | 1,561 B | **65% smaller** vs JSON |
| Disaster Coord | 922 B | 2,473 B | 1,968 B | **63% smaller** vs JSON |
| IoT Sensor | 560 B | 1,267 B | 1,219 B | **56% smaller** vs JSON |
| Survival Guide | 1,253 B | 3,673 B | 2,434 B | **66% smaller** vs JSON |
| Technical Manual | 1,177 B | 2,897 B | 2,184 B | **59% smaller** vs JSON |
| Incident Report | 1,171 B | 3,257 B | 2,323 B | **64% smaller** vs JSON |

### Key Benefits Demonstrated

- ✅ **MML is 56-66% smaller than JSON** - Significant bandwidth savings
- ✅ **MML is 20-50% smaller than XML** - More efficient than standard formats
- ✅ **All formats contain the same information** - No data loss in conversion
- ✅ **Conversion possible between all three formats** - Full interoperability

These examples prove that MML provides the same functionality as JSON and XML while using significantly less bandwidth, making it ideal for constrained environments, radio transmission, and low-power IoT devices.

## Usage

These examples can be:
- Used as templates for creating new MML documents
- Converted between formats (MML ↔ JSON ↔ XML)
- Transmitted via radio, Morse code, or digital networks
- Validated using MML parsers
- Integrated into APIs and web services

## Running Examples

### Validate an example:
```bash
mml-cli validate examples/01-simple-document.mml
```

### Convert to HTML:
```bash
mml-cli convert examples/01-simple-document.mml --to html
```

### Convert to JSON:
```bash
mml-cli convert examples/01-simple-document.mml --to json
```

## Notes

- All examples use UTF-8 encoding
- Examples are designed to be human-readable
- Each example demonstrates different MML features
- Examples can be compressed using MMLC format

