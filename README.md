<div align="center">
&nbsp;</br>
&nbsp;</br>

# > xCDN_
### eXtensible Cognitive Data Notation. The human-first, machine-optimized data serialization format.

[Read the Specification](./docs/SPECIFICATION.md) •
[View Grammar](./grammar/xcdn.ebnf)  •
[See Examples](./examples/README.md)

&nbsp;</br>
&nbsp;</br>
&nbsp;</br>
</div>

## ⚡ Why xCDN?
JSON changed the world, but the world has changed since JSON. xCDN is designed for modern cognitive computing, retaining the simplicity of JSON while adding the semantic richness, streaming capabilities, and types that modern developers actually need.


| Feature | Description |
| :--- | :--- |
| 🧠 **Native Types** | First-class support for `Decimal`, `UUID`, `DateTime`, `Duration`, and `Bytes`. No more string parsing guesswork. |
| 🏷️ **Semantic Tags** | Apply application-level logic with tags like `#money`, `#color`, or `#user`. |
| 📝 **Comments** | finally. `// Single line` and `/* Block comments */`. |
| 🌊 **Streaming** | Built-in support for sequences of values. Perfect for logs and event streams. |
| ✨ **Annotations** | Non-intrusive metadata using `@meta`. Add hints without dirtying your data model. |
| 🔧 **Human Friendly** | Unquoted keys, trailing commas, and multi-line strings (`"""`). |

## 📚 Libraries

Official xCDN parser implementations:

- **Rust**: [xCDN-Rust](https://github.com/gslf/xCDN-Rust)
- **Python**: [xCDN-Python](https://github.com/gslf/xCDN-Python)
- **JavaScript**: [xCDN-JavaScript](https://github.com/gslf/xCDN-JavaScript)
- **C/C++**:  [xCDN-C](https://github.com/gslf/xCDN-C)

## 🚀 Quick Taste
```xCDN
/* ----------------------------- 
    XCDN: Clean, Typed, Powerful.
    ----------------------------  */

$schema: "https://gslf.github.io/xCDN/schemas/v1/meta.xcdn",

server_config: {
  host: "localhost",
  // Unquoted keys & trailing commas? Yes.
  ports: [8080, 9090,], 
  
  // Native Decimals & ISO8601 Duration
  timeout: r"PT30S",
  max_cost: d"19.99",

  // Semantic Tagging
  admin: #user {
    id: u"550e8400-e29b-41d4-a716-446655440000",
    role: "superuser"
  },

  // Binary data handling
  icon: @mime("image/png") b"iVBORw0KGgoAAAANSUhEUgAAAAUA...",
}
```

## 🔌 Integration Details

For parser implementers and HTTP configuration:

| Property | Value |
| :--- | :--- |
| **MIME Type** | `application/xcdn` |
| **File Extension** | `.xcdn` |
| **Magic Bytes** | None (text-based) |
| **Uniform Type Identifier (Apple)** | `org.xcdn.document` |
