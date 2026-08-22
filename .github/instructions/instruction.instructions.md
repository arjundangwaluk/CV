---
description: Apply these instructions when creating, validating, transforming, or refactoring JSON files, JSON Schema definitions, Zod schemas, or data processing scripts.
applyTo: '**/*.{json,jsonl,ndjson,schema.json}'
---

<!-- Tip: Use /create-instructions in chat to generate content with agent assistance -->

### Project Context & Purpose
This project enforces strict data integrity, deterministic schemas, and high-performance serialization for all structured JSON data assets, configuration payloads, and API contracts.

---

### Data Formatting & Syntax Standards

* **Strict JSON Compliance:** All `.json` files must strictly adhere to RFC 8259. Do not use trailing commas, single quotes, unquoted keys, or comments (`//` or `/* */`).
* **Format & Indentation:** Use 2-space indentation for human-readable configuration and test fixture files.
* **Large Datasets:** Datasets containing collections exceeding 50 MB or 10,000 records must use Newline Delimited JSON (`.jsonl` / `.ndjson`) with single-line object serialization.
* **Primitive Types:** Maintain explicit typing across all keys:
  * Numbers must remain numeric (`42`, not `"42"`).
  * Booleans must be raw primitives (`true`/`false`, not `"true"`/`"false"`).
  * Missing or empty states must be either explicitly omitted or set to `null` (never the string `"null"` or `"undefined"`).

---

### Schema & Validation Rules

* **Schema-First Design:** Every payload structure must map to an authoritative JSON Schema (Draft 2020-12) or TypeScript Zod schema located in `/schemas`.
* **Explicit Constraints:** Always specify `additionalProperties: false` on object schemas unless dynamic map structures are explicitly required.
* **Required Properties:** Declare all required keys in the `required` array rather than relying on default application assumptions.
* **Format Enforcements:** Utilize standard schema format specifiers for strings: `date-time`, `uri`, `uuid`, and `email`.

---

### Transformation & Tooling Guidelines

* **Query & Extraction:** Use standard `jq` or JSONPath syntax for data extraction tasks.
* **Safe Streaming:** When writing code that reads or transforms JSON, avoid unbounded in-memory parsing (`JSON.parse()` or `json.loads()` on full files). Use streaming libraries (e.g., `stream-json` in Node.js or `ijson` in Python).
* **Automated Verification:** Any change to schemas or fixtures must be validated against test suites using schema validators (e.g., `ajv-cli` or Zod test suites) before committing.