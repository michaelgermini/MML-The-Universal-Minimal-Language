# Appendix E — Minimal MML Parser Implementation

## Reference implementations

This appendix provides links to minimal MML parser implementations in various programming languages. These implementations demonstrate the simplicity of parsing MML and serve as reference code.

### Available implementations

See the [implementations directory](../implementations/) for complete parser implementations in:

- **JavaScript**: Complete parser with DOM support
- **Python**: Full-featured library with validation
- **C++**: Lightweight embedded implementation
- **Rust**: High-performance parser
- **Go**: Cloud-ready service implementation

### Minimal parser example

#### JavaScript (15 lines)
```javascript
function parseMML(text) {
  const lines = text.split('\n');
  const doc = { nodes: [] };

  for (const line of lines) {
    if (line.includes(':')) {
      const [tag, content] = line.split(':', 2);
      doc.nodes.push({ tag: tag.trim(), content: content.trim() });
    }
  }

  return doc;
}
```

#### Python (12 lines)
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

### Implementation principles

#### Simplicity first
- **Line-by-line parsing**: Each line is independent
- **Tag detection**: Simple colon separator
- **Error tolerance**: Invalid lines ignored

#### Extensibility
- **Modular design**: Easy to add new tags
- **DOM support**: Optional tree structure
- **Validation**: Optional schema checking

### Complete implementations

For production-ready parsers with full DOM support, validation, and conversion capabilities, see:

- [JavaScript implementation](../implementations/mml-parser.js)
- [Python implementation](../implementations/mml_parser.py)
- [C++ implementation](../implementations/cpp/)
- [Rust implementation](../implementations/rust/)
- [Go implementation](../implementations/go/)

---

**These minimal implementations demonstrate that MML parsing can be accomplished in just a few lines of code, making it accessible even for embedded systems and educational purposes.**

