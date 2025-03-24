---

title: "JSON Structure Alternate Names and Descriptions"
category: info

docname: draft-vasters-httpapi-json-structure-alternate-names-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date: 2025-04-24
consensus: true
v: 3
area: ""
workgroup: "Building Blocks for HTTP APIs"
keyword:
 - JSON
 - schema
venue:
  group: "Building Blocks for HTTP APIs"
  type: ""
  mail: "httpapi@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/httpapi/"
  github: "json-structure/alternate-names"
  latest: "https://json-structure.github.io/alternate-names/draft-vasters-httpapi-json-structure-alternate-names.html"

author:
 -
    fullname: Clemens Vasters
    organization: Microsoft Corporation
    email: clemensv@microsoft.com

normative:
  RFC2119:
  RFC4646:
  RFC8174:
  JSTRUCT-CORE:
    title: "JSON Structure Core"
    author:
      - fullname: Clemens Vasters
    target: https://json-structure.github.io/core/draft-vasters-httpapi-json-structure-core.html

informative:


--- abstract

This document is an extension to JSON Structure Core. It defines three
annotation keywords, `altnames`, `altenums`, and `descriptions`, which allow
schema authors to provide alternative identifiers, display names, and
multi-variant descriptions for types, properties, and enumeration values.


--- middle

## Introduction {#introduction}

This document is an extension to JSON Structure Core {{JSTRUCT-CORE}}. It
defines three annotation keywords, `altnames`, `altenums`, and `descriptions`,
which allow schema authors to provide alternative identifiers, display names,
and multi-variant descriptions for types, properties, and enumeration values.

These annotations facilitate mapping between internal schema identifiers and
external data representations (e.g., JSON keys that do not conform to identifier
rules) and support internationalization by enabling localized labels.

## Conventions {#conventions}

{::boilerplate bcp14}

## Keywords {#keywords}

This section defines the alternate names and symbols annotations.

### The `altnames` Keyword {#the-altnames-keyword}

The `altnames` keyword provides alternative names for a named type or property.
Alternate names are not subject to the identifier syntax restrictions imposed on
`name` and MAY be used to map internal schema names to external representations.

The value of `altnames` MUST be a map where each key is a purpose indicator and
each value is a string representing an alternate name.

- The key `"json"` is RESERVED. When used, it specifies the property key to use
  for a property when encoded in JSON. This allows for JSON keys that do not
  conform to identifier rules.
- Keys starting with the prefix `"lang:"` (e.g., `"lang:en"`, `"lang:fr"`) are
  RESERVED for localized display names. The suffix after the colon specifies the
  language code. The language code MUST conform to {{RFC4646}}.
- Other keys are allowed for custom usage, provided they do not conflict with
  the reserved keys or prefixes.

The `altnames` keyword MAY be included in any schema element that has an
explicit name (i.e., named types or properties). When present, the alternate
names provide additional mappings that schema processors MAY use for encoding,
decoding, or user interface display.

Example:

~~~json
{
  "Person": {
    "type": "object",
    "altnames": {
      "json": "person_data",
      "lang:en": "Person",
      "lang:de": "Person"
    },
    "properties": {
      "firstName": {
        "type": "string",
        "altnames": {
          "json": "first_name",
          "lang:en": "First Name",
          "lang:de": "Vorname"
        }
      },
      "lastName": {
        "type": "string",
        "altnames": {
          "json": "last_name",
          "lang:en": "Last Name",
          "lang:de": "Nachname"
        }
      }
    },
    "required": ["firstName", "lastName"]
  }
}
~~~

### The `altenums` Keyword {#the-altenums-keyword}

The `altenums` keyword provides alternative representations (symbols) for
enumeration values defined by a type using the `enum` keyword. Alternate symbols
allow schema authors to map internal enum values to external codes or localized
display symbols.

The value of `altenums` MUST be a JSON object (map) where each key is a purpose
indicator and each corresponding value is an object mapping each enumeration
value (as defined in the `enum` array) to its alternate symbol.

- The key `"json"` is RESERVED and MUST be used to specify alternate symbols for
  JSON encoding.
- Keys beginning with `"lang:"` (e.g., `"lang:en"`, `"lang:es"`) are RESERVED
  for providing localized alternate symbols.
- Additional keys are permitted for custom usage, subject to no conflicts with
  reserved keys or prefixes.

The `altenums` keyword MUST be used only with schemas that include an `enum`
keyword. When present, it provides alternative representations for each
enumeration value that schema processors MAY use for encoding or display
purposes.

~~~json
{
  "Color": {
    "type": "string",
    "enum": ["RED", "GREEN", "BLUE"],
    "altenums": {
      "json": {
        "RED": "#FF0000",
        "GREEN": "#00FF00",
        "BLUE": "#0000FF"
      },
      "lang:en": {
        "RED": "Red",
        "GREEN": "Green",
        "BLUE": "Blue"
      },
      "lang:de": {
        "RED": "Rot",
        "GREEN": "Grün",
        "BLUE": "Blau"
      }
    }
  }
}
~~~

### The `descriptions` Keyword {#the-descriptions-keyword}

The `descriptions` keyword provides multi-variant descriptions as an alternative
to the `description` keyword. It allows schema authors to provide localized or
context-specific descriptions for types, properties, or enumeration values.

The value of `descriptions` MUST be a map where each key is a purpose indicator
and each value is a string representing a description.

- Keys beginning with `"lang:"` (e.g., `"lang:en"`, `"lang:fr"`) are RESERVED
  for localized descriptions. The suffix after the colon specifies the language
  code. The language code MUST conform to {{RFC4646}}.
- Other keys are allowed for custom usage, provided they do not conflict with
  the reserved keys or prefixes.

The `descriptions` keyword MAY be included in any schema element that can have a
description. When present, the descriptions provide additional mappings that
schema processors MAY use for user interface display.

Example:

~~~json
{
  "Person": {
    "type": "object",
    "descriptions": {
      "lang:en": "A person object with first and last name",
      "lang:de": "Ein Person-Objekt mit Vor- und Nachnamen",
      "lang:fr": "Un objet personne avec prénom et nom de famille",
      "lang:cn": "一个带有名字和姓氏的人对象",
      "lang:jp": "名前と姓を持つ人物オブジェクト"
    },
    "properties": {
      "firstName": {
        "type": "string",
        "descriptions": {
          "lang:en": "The first name of the person",
          "lang:de": "Der Vorname der Person",
          "lang:fr": "Le prénom de la personne",
          "lang:cn": "人的名字",
          "lang:jp": "人の名前"
        }
      },
      "lastName": {
        "type": "string",
        "descriptions": {
          "lang:en": "The last name of the person",
          "lang:de": "Der Nachname der Person",
          "lang:fr": "Le nom de famille de la personne",
          "lang:cn": "人的姓氏",
          "lang:jp": "人の姓"
        }
      }
    },
    "required": ["firstName", "lastName"]
  }
}
~~~

## Security and Interoperability Considerations {#security-and-interoperability-considerations}

Alternate names and symbols annotations do not affect the validation of instance
data. They are purely metadata and MUST be ignored by validators that do not
support this extension. However, applications that rely on alternate names for
mapping or localization MUST implement appropriate safeguards to ensure that the
alternate identifiers are used consistently.

## IANA Considerations {#iana-considerations}

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
