# Chapter 8 — MML Converters

## 8.1 MML → HTML

### Conversion to modern web

#### Objective
Transform an MML document into a standard HTML page for web display.

#### Semantic mapping
- **T:** → `<h1>`
- **H:** → `<h2>`, `<h3>`, etc.
- **P:** → `<p>`
- **L:** → `<a href>`
- **IMG:** → `<img src>`
- **M:** → `<meta>` or `data-` attributes
- **Q:** → `<blockquote>`

#### Conversion example

**MML**:
```
T:Survival guide
H:Food
P:In survival situations, water is more important than food.
L:Complete guide|https://survival.org
```

**Generated HTML**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Survival guide</title>
</head>
<body>
    <h1>Survival guide</h1>
    <h2>Food</h2>
    <p>In survival situations, water is more important than food.</p>
    <p><a href="https://survival.org">Complete guide</a></p>
</body>
</html>
```

### JavaScript implementation

```javascript
class MMLToHTMLConverter {
  convert(mmlText) {
    const dom = this.parseMML(mmlText);
    return this.generateHTML(dom);
  }

  generateHTML(dom) {
    let html = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>${this.escapeHTML(dom.title || 'MML Document')}</title>
</head>
<body>`;

    if (dom.title) {
      html += `<h1>${this.escapeHTML(dom.title)}</h1>`;
    }

    for (const section of dom.sections) {
      html += `<h2>${this.escapeHTML(section.title)}</h2>`;
      for (const child of section.children) {
        html += this.convertNodeToHTML(child);
      }
    }

    html += '</body></html>';
    return html;
  }

  convertNodeToHTML(node) {
    switch (node.type) {
      case 'paragraph':
        return `<p>${this.escapeHTML(node.content)}</p>`;
      case 'link':
        return `<p><a href="${this.escapeHTML(node.url)}">${this.escapeHTML(node.text)}</a></p>`;
      case 'quote':
        return `<blockquote>${this.escapeHTML(node.content)}</blockquote>`;
      default:
        return `<p>${this.escapeHTML(node.content)}</p>`;
    }
  }

  escapeHTML(text) {
    return text
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  }
}
```

## 8.2 MML → JSON

### Structured serialization

#### Advantages
- **API integration**: REST web services
- **Database storage**: NoSQL documents
- **Algorithmic processing**: Automated analysis

#### Generated JSON structure
```json
{
  "type": "document",
  "title": "Survival guide",
  "metadata": {
    "author": "Survival team",
    "version": "1.0"
  },
  "sections": [
    {
      "title": "Food",
      "metadata": {},
      "children": [
        {
          "type": "paragraph",
          "content": "Water is essential"
        }
      ]
    }
  ]
}
```

## 8.3 MML → plain text

### Readable content extraction

#### Usage
- **Printing**: Paper documents
- **Archiving**: Simple text
- **Accessibility**: Screen readers

#### Output format
```
SURVIVAL GUIDE
================

FOOD
------------
Water is essential for human survival.

RESOURCES
----------
- Complete guide: https://survival.org
```

## 8.4 MML → Morse

### Translation for radio transmission

#### Morse mapping
- **T:** → - (Title)
- **H:** → .... (Header)
- **P:** → .--. (Paragraph)
- **Space** → / (word separation)

#### Example
**MML**: `T:Alert P:Evacuate`

**Morse**: `- / .- .-.. . .-./ .--. / . ...- .- -.-. ..- .--..`

## 8.5 MML → DNF

### Packaging for transport

#### Automatic generation
Each MML line becomes a DNF fragment with header.

#### Format
```
DNF:SEQ=1:TOTAL=5:ID=MSG-001
T:Security alert

DNF:SEQ=2:TOTAL=5:ID=MSG-001
H:Compromised zone

DNF:SEQ=3:TOTAL=5:ID=MSG-001
M:Severity|Critical
```

## 8.6 Ultra-lightweight embedded interpreters

### MML on microcontrollers

#### Constraints
- **RAM**: 2-64 KB
- **Flash**: 32-512 KB
- **CPU**: 8-32 MHz

#### Arduino implementation
```cpp
class MMLParser {
private:
  char buffer[256];
  int bufferPos = 0;

public:
  void parse(char* mmlText) {
    char* line = strtok(mmlText, "\n");
    while (line != NULL) {
      parseLine(line);
      line = strtok(NULL, "\n");
    }
  }

  void parseLine(char* line) {
    char* colonPos = strchr(line, ':');
    if (colonPos != NULL) {
      *colonPos = '\0'; // Separate tag and content
      char* tag = line;
      char* content = colonPos + 1;

      // Process according to tag
      if (strcmp(tag, "T") == 0) {
        Serial.print("Title: ");
        Serial.println(content);
      } else if (strcmp(tag, "P") == 0) {
        Serial.print("Para: ");
        Serial.println(content);
      }
      // ... other tags
    }
  }
};
```

### IoT applications

#### Sensors transmitting MML
- **Temperature**: `M:Temp|23.5°C`
- **Humidity**: `M:Hum|65%`
- **Battery**: `M:Bat|78%`

#### Sensor network
```
PKT:SENSOR-NET-001
T:Environmental data
M:Station|EXT-01
M:Temp|22.3
M:Hum|67
M:Pressure|1013.2
END
```

## 8.7 Libraries and implementations (JS, Python, PHP, C)

### Development ecosystem

#### JavaScript/Node.js
- **mml-parser**: Complete parser with DOM
- **mml-to-html**: Web converter
- **mml-dnf**: Network integration

#### Python
```python
from mml_parser import MMLParser

parser = MMLParser()
document = parser.parse("""
T:Report
H:Section 1
P:Content
""")

print(document.title)  # "Report"
print(document.sections[0].title)  # "Section 1"
```

#### PHP (web applications)
```php
$mml = new MMLConverter();
$html = $mml->toHTML($mmlContent);
echo $html; // Display HTML page
```

#### C (embedded systems)
- **LibMML**: Lightweight library
- **No dependencies**: Works without OS
- **Fixed memory**: Predictable

### Development tools

#### Online validation
Web interface to test and validate MML documents.

#### Automatic generators
- **AI → MML**: Automatic conversion from natural text
- **MML → Diagrams**: Schema generation
- **MML → PDF**: Printable documents

---

**Chapter conclusion**: MML converters open the format to all existing ecosystems. Whether displaying a document on the web, integrating it into a REST API, or processing it on a microcontroller, MML can be transformed into the target format without information loss. This universal interoperability makes MML much more than a storage format – it's a cross-cutting communication protocol.

