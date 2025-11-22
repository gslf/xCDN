# > xCDN_ Structural Specification

This document defines the data model, semantics, and structural rules for the eXtensible Cognitive Data Notation (XCDN).

> **Note:** For the formal Extended Backus-Naur Form (EBNF) definition, please refer to the **[Grammar File](../grammar/xcdn.ebnf)**.

## 1. Essential Semantic Notes

* **Duplicate keys** in an `object` are strictly forbidden and must result in a validation error.
* **Trailing commas** are permitted in both `object` and `array` definitions to facilitate code generation and editing.
* **Annotations** (`@name`) can precede **any** `primary` value. They may appear multiple times, and their order is preserved.
* **Tags** (`#name`) apply an **application-level namespace** to any value.
* **Quoted Payloads** for prefixed types (`d""`, `b""`, `t""`, `r""`, `u""`) are validated according to their respective standards (Decimal, Base64, RFC3339, ISO8601 Duration, UUID).

---

## 2. Structural Sheet

### 2.1. Document Structure

An XCDN document is a text file (encoded in UTF-8) that consists of two main parts:

1.  **Optional Prolog:** A set of **directives** at the very beginning of the document. Directives are prefixed with `$` (e.g., `$schema: "..."`) and provide document-level metadata or processing instructions.
2.  **Value Sequence:** A sequence of **one or more complete XCDN values**.

Values in the sequence are separated by insignificant whitespace (including newlines and comments). This structure inherently supports **streaming**: a document can represent a single, static configuration (one value) or a continuous stream of distinct values (e.g., log entries, events).

### 2.2. The Data Model

The fundamental unit of data in XCDN is the **value**. Every value is composed of an optional set of annotations followed by a single primary value. The primary value categories are: **scalar**, **composite**, or **tagged**.

#### Scalar (Primitive) Values

Scalars represent single, indivisible data points.

| Type | Syntax | Description |
| :--- | :--- | :--- |
| **Null** | `null` | Represents the absence of a value. |
| **Boolean** | `true`, `false` | Logical truth values. |
| **Number** | `42`, `-3.14`, `1.2e6` | Standard 64-bit floating-point numbers. |
| **String** | `"text"`, `"""multi"""` | UTF-8 characters. Supports JSON-style escapes and multi-line literals. |

#### Typed Literals (Prefixed Scalars)

For data types that require precise, non-ambiguous representation, XCDN provides a set of prefixed, string-based literals.

* **Decimal** (`d"..."`): Arbitrary-precision decimal number. The string payload is preserved for exact scale.
    * *Example:* `d"1234567890.001200"`
* **Bytes** (`b"..."`): Binary data, encoded as a Base64 (standard or URL-safe) string.
    * *Example:* `b"SGVsbG8="`
* **DateTime** (`t"..."`): A specific point in time, formatted as an RFC 3339 timestamp string.
    * *Example:* `t"2025-10-05T12:34:56Z"`
* **Duration** (`r"..."`): A length of time, formatted as an ISO 8601 duration string.
    * *Example:* `r"PT2H30M"`
* **UUID** (`u"..."`): A Universally Unique Identifier.
    * *Example:* `u"550e8400-e29b-41d4-a716-446655440000"`

#### Composite (Structural) Values

Composites are containers that hold other values.

* **Object** (`{ ... }`): An **unordered** collection of key-value pairs. Keys must be unique within the object and may be a `string` or an unquoted `ident` (identifier).
* **Array** (`[ ... ]`): An **ordered** sequence (list) of values. Types may be mixed within the array.

### 2.3. Metadata and Extensibility

XCDN includes two distinct mechanisms for adding context to data:

1.  **Tagged Values (Semantic):**
    * Syntax: `#ident value` (e.g., `#money { ... }`).
    * Tags are part of the data model. They distinguish logical types (e.g., a generic object vs. a `#user` object).
2.  **Annotations (Non-Semantic):**
    * Syntax: `@ident(args) value` (e.g., `@deprecated`).
    * Annotations provide metadata *about* a value (parser hints, validation rules) without altering the value itself.

### 2.4. Syntactic Features

* **Comments:** Single-line (`//`) and multi-line (`/* */`) are supported.
* **Identifiers:** Keys, tags, and annotation names can be unquoted `ident`s (e.g., `my-key: "value"`) for readability.
* **Whitespace:** Flexible formatting; space, tabs, and newlines are insignificant between tokens.

### 2.5. Streaming

The EBNF rule `document = ... value { WS? value }* ...` formally defines the streaming capability.

* **Static Document:** A file containing exactly **one** top-level value.
* **Stream:** A file or data flow containing **multiple** top-level values.

**Streaming Logic:**
1.  Parser reads optional prolog.
2.  Parser reads the first complete value.
3.  Value is emitted.
4.  Parser discards intervening whitespace/comments.
5.  Repeat until EOF.