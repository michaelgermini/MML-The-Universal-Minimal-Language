# Chapter 4 — The MML DOM

## 4.1 Why a DOM for a minimal language?

### Beyond plain text

MML, despite its simple syntax, must be processable in a structured way by computer programs. The **MML DOM** (Document Object Model) provides this logical structure.

#### Problem
An MML document is initially a text stream:
```
T:My document
H:Section 1
P:Content
```

**Need**: Transform this stream into a structure usable by programs.

#### Solution: The MML DOM
A tree representation of the document that:
- Respects tag semantics
- Allows programmatic navigation
- Facilitates transformations (HTML, JSON, etc.)
- Supports validation and queries

### Advantages of structured DOM

#### 1. Programmatic processing
```javascript
// Access elements by type
const titles = dom.querySelectorAll('T');
const sections = dom.querySelectorAll('H');
```

#### 2. Automatic transformations
```javascript
// MML → HTML conversion
const html = dom.toHTML();
// MML → JSON conversion
const json = dom.toJSON();
```

#### 3. Semantic validation
```javascript
// Structure verification
const isValid = dom.validate();
```

## 4.2 Building a logical tree

### Construction principle

#### Basic algorithm
1. **Parse line by line** the MML document
2. **Create a node** for each valid element
3. **Establish hierarchy** based on tag types
4. **Manage metadata** as node attributes

#### Transformation example

**MML Document**:
```
T:Survival guide
M:Author|John Doe
H:Chapter 1
P:Introduction to the subject
H:Chapter 2
P:Chapter 2 content
```

**Resulting DOM tree**:
```
Document
├── title: "Survival guide"
├── metadata:
│   └── author: "John Doe"
├── sections:
│   ├── Section 1
│   │   ├── title: "Chapter 1"
│   │   └── content: "Introduction to the subject"
│   └── Section 2
│       ├── title: "Chapter 2"
│       └── content: "Chapter 2 content"
```

### Implicit vs explicit hierarchy

#### MML implicit hierarchy
Structure is determined by order and tag types:
- **T:** → Document root
- **H:** → Level 1 sections
- **P:** → Section content
- **M:** → Associated metadata

#### Tree construction
```javascript
class MMLDOMBuilder {
  constructor() {
    this.document = { type: 'document', children: [] };
    this.currentSection = null;
  }

  addNode(tag, content) {
    const node = { type: tag.toLowerCase(), content };

    switch(tag) {
      case 'T':
        this.document.title = content;
        break;
      case 'H':
        this.currentSection = { type: 'section', title: content, children: [] };
        this.document.children.push(this.currentSection);
        break;
      case 'P':
        if (this.currentSection) {
          this.currentSection.children.push({ type: 'paragraph', content });
        }
        break;
      case 'M':
        const [key, value] = content.split('|', 2);
        this.document.metadata = this.document.metadata || {};
        this.document.metadata[key.trim()] = value.trim();
        break;
    }
  }
}
```

## 4.3 Parallels with HTML / XML / JSON DOM

### Comparison with HTML DOM

#### Traditional HTML DOM
```html
<!DOCTYPE html>
<html>
<head><title>My title</title></head>
<body>
  <h1>Section 1</h1>
  <p>Paragraph 1</p>
  <h1>Section 2</h1>
  <p>Paragraph 2</p>
</body>
</html>
```

**Rigid tree structure** with explicit nesting.

#### Equivalent MML DOM
```
T:My title
H:Section 1
P:Paragraph 1
H:Section 2
P:Paragraph 2
```

**Tree structure reconstructed** from a flat stream.

### Comparison with XML DOM

#### XML with explicit structure
```xml
<document>
  <title>My title</title>
  <section>
    <title>Section 1</title>
    <paragraph>Paragraph 1</paragraph>
  </section>
</document>
```

#### MML with implicit structure
```
T:My title
H:Section 1
P:Paragraph 1
```

**Identical DOM result**, but MML syntax 60% more concise.

### Comparison with JSON

#### Rigid structural JSON
```json
{
  "title": "My title",
  "sections": [
    {
      "title": "Section 1",
      "content": "Paragraph 1"
    }
  ]
}
```

#### More flexible MML
MML DOM allows:
- **Dynamic addition** of properties
- **Extensible metadata**
- **Adaptable structure** according to needs

## 4.4 Section detection

### Hierarchical reconstruction algorithm

#### Hierarchy rules

1. **H: create new sections**
2. **P: belong to the previous H: section**
3. **M: can be global or associated with current section**
4. **L: and other elements are contextual**

#### Complex example

**MML Document**:
```
T:Technical manual
M:Author|Technical team
H:Installation
M:Duration|2 hours
P:System prerequisites
P:Installation procedure
H:Configuration
P:Basic settings
H:Security
M:Level|Advanced
P:SSL configuration
```

**Reconstructed hierarchy**:
```
Technical manual
├── Global metadata: Author
├── Section: Installation
│   ├── Metadata: Duration
│   ├── Paragraph: System prerequisites
│   └── Paragraph: Installation procedure
├── Section: Configuration
│   └── Paragraph: Basic settings
└── Section: Security
    ├── Metadata: Level
    └── Paragraph: SSL configuration
```

### Nested section handling

#### Subsection problem
MML has only one H: tag, how to handle hierarchical levels?

#### Solutions
1. **Naming convention**: "Chapter 1", "Section 1.1"
2. **Semantic analysis**: Automatic level detection
3. **Explicit metadata**: M:Level|2

#### Subsection example
```
H:Chapter 1: The basics
H:Section 1.1: Introduction
H:Section 1.2: Fundamental concepts
H:Chapter 2: Advanced applications
```

## 4.5 Hierarchical tree reconstructed from flat stream

### Complete reconstruction algorithm

#### JavaScript implementation
```javascript
class MMLParser {
  constructor() {
    this.reset();
  }

  reset() {
    this.dom = {
      type: 'document',
      metadata: {},
      sections: []
    };
    this.currentSection = null;
    this.sectionStack = [];
  }

  parse(text) {
    this.reset();
    const lines = text.split('\n');

    for (const line of lines) {
      const node = this.parseLine(line.trim());
      if (node) {
        this.addNode(node);
      }
    }

    return this.dom;
  }

  parseLine(line) {
    if (!line || !line.includes(':')) return null;

    const [tag, content] = line.split(':', 2);
    return {
      tag: tag.trim().toUpperCase(),
      content: content.trim()
    };
  }

  addNode(node) {
    switch(node.tag) {
      case 'T':
        this.dom.title = node.content;
        break;

      case 'M':
        const [key, value] = node.content.split('|', 2);
        const metadata = this.currentSection ?
          (this.currentSection.metadata = this.currentSection.metadata || {}) :
          this.dom.metadata;
        metadata[key.trim()] = value ? value.trim() : '';
        break;

      case 'H':
        const section = {
          type: 'section',
          title: node.content,
          metadata: {},
          children: []
        };
        this.dom.sections.push(section);
        this.currentSection = section;
        break;

      case 'P':
        if (this.currentSection) {
          this.currentSection.children.push({
            type: 'paragraph',
            content: node.content
          });
        }
        break;

      case 'L':
        const [linkText, linkUrl] = node.content.split('|', 2);
        const linkNode = {
          type: 'link',
          text: linkText.trim(),
          url: linkUrl ? linkUrl.trim() : ''
        };

        if (this.currentSection) {
          this.currentSection.children.push(linkNode);
        } else {
          // Global link
          this.dom.links = this.dom.links || [];
          this.dom.links.push(linkNode);
        }
        break;

      // Handle other tags...
    }
  }
}
```

### Error handling and resilience

#### Invalid lines
Lines that don't follow the TAG:content format are silently ignored.

#### Error recovery
- Partially corrupted documents remain usable
- Incomplete sections are preserved
- Orphaned metadata are attached to global document

## 4.6 Resilience to line losses

### Robustness test

#### Scenario: 30% line loss
**Original document**:
```
T:Emergency report
H:Sector A
M:Victims|5
P:Stable status
H:Sector B
M:Victims|3
P:Reinforcements needed
```

**After losses**:
```
T:Emergency report
H:Sector A
P:Stable status
H:Sector B
P:Reinforcements needed
```

**Reconstructed DOM**: Functional despite metadata losses.

#### Scenario: Mixed lines
**Disturbed order**:
```
H:Sector B
T:Emergency report
P:Stable status
H:Sector A
P:Reinforcements needed
```

**Reconstruction**: Parser can logically reorganize content.

### Resilience strategies

#### 1. Loose validation
Accept incomplete documents rather than rejecting them.

#### 2. Heuristic reconstruction
Use rules to attach orphaned elements.

#### 3. Redundant metadata
Repeat important information in multiple formats.

## 4.7 Formal definition of DOM nodes

### Basic node schema

#### Document node
```javascript
{
  type: "document",
  title: string,
  metadata: {[key: string]: string},
  sections: Section[],
  links: Link[]
}
```

#### Section node
```javascript
{
  type: "section",
  title: string,
  metadata: {[key: string]: string},
  children: (Paragraph | Link | Image | Code | Quote)[]
}
```

#### Content nodes
```javascript
// Paragraph
{
  type: "paragraph",
  content: string
}

// Link
{
  type: "link",
  text: string,
  url: string
}

// Image
{
  type: "image",
  description: string,
  url: string
}

// Code
{
  type: "code",
  content: string,
  language?: string
}

// Quote
{
  type: "quote",
  content: string
}
```

### Schema extensions

#### Specialized nodes
```javascript
// Configuration
{
  type: "configuration",
  section: string,
  parameters: {[key: string]: string}
}

// DNF packet
{
  type: "packet",
  id: string,
  children: MMLNode[]
}
```

## 4.8 Complete MML DOM example in JSON

### Example MML document

```
T:First aid guide
M:Author|Red Cross
M:Version|2.1
M:Last revision|2025-11-15

H:Cardiac arrest
M:Severity|Critical
P:In case of cardiac arrest, act immediately.
P:1. Check consciousness
P:2. Call emergency services
P:3. Begin cardiac massage

Q:Remember: persist until emergency services arrive

L:Demonstration video|https://example.com/cpr-video
IMG:Massage diagram|cardiac-massage.png

H:Choking
P:For a conscious adult:
P:1. Encourage coughing
P:2. If failure, give 5 back blows
P:3. Alternate with 5 abdominal compressions

C:# Code to calculate massage frequency
C:frequency = 100 // compressions per minute
C:depth = 5 // centimeters
```

### Resulting DOM JSON

```json
{
  "type": "document",
  "title": "First aid guide",
  "metadata": {
    "Author": "Red Cross",
    "Version": "2.1",
    "Last revision": "2025-11-15"
  },
  "sections": [
    {
      "type": "section",
      "title": "Cardiac arrest",
      "metadata": {
        "Severity": "Critical"
      },
      "children": [
        {
          "type": "paragraph",
          "content": "In case of cardiac arrest, act immediately."
        },
        {
          "type": "paragraph",
          "content": "1. Check consciousness"
        },
        {
          "type": "paragraph",
          "content": "2. Call emergency services"
        },
        {
          "type": "paragraph",
          "content": "3. Begin cardiac massage"
        },
        {
          "type": "quote",
          "content": "Remember: persist until emergency services arrive"
        },
        {
          "type": "link",
          "text": "Demonstration video",
          "url": "https://example.com/cpr-video"
        },
        {
          "type": "image",
          "description": "Massage diagram",
          "url": "cardiac-massage.png"
        }
      ]
    },
    {
      "type": "section",
      "title": "Choking",
      "metadata": {},
      "children": [
        {
          "type": "paragraph",
          "content": "For a conscious adult:"
        },
        {
          "type": "paragraph",
          "content": "1. Encourage coughing"
        },
        {
          "type": "paragraph",
          "content": "2. If failure, give 5 back blows"
        },
        {
          "type": "paragraph",
          "content": "3. Alternate with 5 abdominal compressions"
        },
        {
          "type": "code",
          "content": "# Code to calculate massage frequency"
        },
        {
          "type": "code",
          "content": "frequency = 100 // compressions per minute"
        },
        {
          "type": "code",
          "content": "depth = 5 // centimeters"
        }
      ]
    }
  ],
  "links": []
}
```

---

**Chapter conclusion**: The MML DOM transforms a simple text stream into a logical structure usable by programs. This abstraction allows MML to benefit from the advantages of a structured format while preserving its transmission simplicity and resilience. The MML DOM is the key that opens MML to the modern programming world while preserving its minimalist essence.

