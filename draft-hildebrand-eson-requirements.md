---
title: Extended Scripting Object Notation (ESON) Requirements
abbrev: ESON Requirements
category: std

docname: draft-hildebrand-eson-requirements-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
# area: art
# workgroup: dispatch
keyword:
 - json
 - text
 - scripting
venue:
#  group: dispatch
#  type: Working Group
#  mail: dispatch@ietf.org
#  arch: https://datatracker.ietf.org/wg/dispatch/
  github: "hildjj/draft-hildebrand-eson-requirements"
  latest: "https://hildjj.github.io/draft-hildebrand-eson-requirements/draft-hildebrand-eson-requirements.html"

author:
 -
    fullname: Joe Hildebrand
    organization: Fractional CTO Associates
    email: joe-ietf@cursive.net

normative:
  RFC3339:
  RFC7493:
  RFC8259:

informative:
  Temporal:
    target: https://tc39.es/proposal-temporal/
    title: ECMAScript Temporal proposal
    date: 2024-01-22
    seriesinfo:
      ECMA: TC39

--- abstract

Requirements for a new data interchange format as extensions to JSON, ensuring
that all existing I-JSON would be valid ESON, adding features for usability.

--- middle

# Introduction

JSON [RFC8259] was originally designed as a format for interchanging a
simplified subset of the JavaScript language between different endpoints.  It
has since come to be used in many places that were not originally targeted in
the original design.  JavaScript (now ECMAScript) is far from the only language
that is using JSON, and we no longer parse JSON by throwing it at a generic
JavaScript runtime using `eval`.

ESON aspires to be a language-neutral, text-based format that fulfills similar
missions as JSON has, while adding better definition for the edge cases that
have surfaced from years of JSON use, allowing enhanced developer ergonomics,
and adding data types that would be useful for interchange.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Requirements

1. All existing I-JSON ([RFC7493]) documents SHALL be valid ESON.
1. All minor errors in parsing ESON, including ranges for numbers, SHALL cause
   a parse error.
1. ESON is always encoded as UTF-8 for interchange.  Note that this explicitly
   means that bare code points in the surrogate range (D800-DFFF) MUST cause
   an error when encountered at any point in the input, even when escaped.
1. ESON SHALL have a comment syntax that is valid at any point before or after
   any token.  Both single- and multi-line comments in the style of ECMAScript
   and C are envisioned.
1. Object keys MAY be unquoted Identifiers in the form
   `/\p{ID_Start}\p{ID_Continue}*/u`.
1. Commas MAY be skipped between array or object items as long as there is
   whitespace between the items, and MAY occur after the last array or object
   item.
1. Strings MAY use either single quotes or double quotes.  Double quotes need
   not be escaped inside single quotes, and vice-versa (as is currently the
   case in JSON).
1. Strings MAY span multiple lines by including newline characters.  These
   newline characters are a part of the encoded information.
1. Strings MAY include Unicode escapes for any code point (e.g. `"\u{1F4A9}"`).
1. A number space that includes at least all 64-bit signed or unsigned
   integers SHALL be defined.  An arbitrary-length number space MAY be
   defined. These spaces might be continuous or not, but the semantics of the
   interactions and overlaps between them SHALL be specified.
1. Numbers MAY be encoded in hexadecimal with the `0x` prefix.
1. Numbers MAY have a leading or trailing decimal point.  The semantics for a
   trailing decimal point SHALL be explicitly defined.
1. The numbers `Infinity`, `-Infinity`, and `NaN` SHALL be valid.
1. Numbers outside the range of IEEE754-2019's binary64 type MUST cause a
   parse error.
1. Numbers MAY have a leading `+` sign.
1. Any whitespace character with the Unicode class `Zs` SHALL be valid
   whitespace.
1. Duplicate object keys MUST cause a parse error.
1. Type extensibility is desired.  If possible, the work done for CBOR tags
   should be used.  Semantics SHALL be defined for parsing behavior when
   the receiving entity does not implement a particular type extension.
1. There SHALL be a date type that allows encoding at least all of the
   information from [RFC3339].  See the [Temporal] proposal for other ideas.
   If there is a type extensibility approach, the date type SHALL use it.
1. The SHALL be a base64 type that allows explicit interchange of binary data.
   If there is a type extensibility approach, the base64 type SHALL use it.

# Security Considerations

Canonicalization and deterministic encoding SHALL NOT be defined for this
format.  As such, it SHOULD NOT be used for cryptographic-adjacent protocols
that require these features.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
