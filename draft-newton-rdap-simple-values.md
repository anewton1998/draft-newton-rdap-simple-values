---
title: "RDAP Simple Values"
abbrev: "RDAP Simple Values"
category: std

docname: draft-newton-rdap-simple-values-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date: {DATE}
consensus: true
v: 3
# area: ART
# workgroup: REGEXT Working Group
keyword:
 - RDAP
venue:
#  group: REGEXT
#  type: Working Group
#  mail: regext@ietf.org
#  arch: https://mailarchive.ietf.org/arch/browse/regext/
  github: "anewton1998/draft-newton-rdap-simple-values"
  latest: "https://anewton1998.github.io/draft-newton-rdap-simple-values/draft-newton-rdap-simple-values.html"

author:
 -
    fullname: Andy Newton
    organization: ICANN
    email: andy@hxr.us

normative:

informative:

...

--- abstract

This document defines an extension for the Registration Data Access Protocol
to express simple values for scenarios where specific data is needed, but
the format of the data is not complicated enough to require a specific
extension for that data.

--- middle

# Introduction

The Registration Data Access Protocol (RDAP) defines a core set of
data types for Domain Name Registries (DNRs) and Internet Number
Resource Registries (INRRs) as defined in {{!RFC9083}}. When a registry
needs to express data not found in {{!RFC9083}}, an RDAP extension
must be used for the expression of that data. However, RDAP extensions
require changes to client software to be useful by most users. For
scenarios in which the data is not complex, this can be overly burdensome.

This RDAP extension defines a method for the expression of simple values
that is reusable across multiple use cases.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Simple Value Data

This RDAP extension defines a JSON object of the following form:

```json
"simpleValues_data" : [
  {
    "name" : "example",
    "value" : "the data",
    "scope" : "some scope",
    "links" : [
       ... (links from RFC 9083)
    ]
  }
]
```

This JSON object contains an array of name/value JSON objects, each with the following members:

* "name" - a string containing the registered name of the value.
* "value" - depending on the registration of the name, this is one of the following data types:
  * a JSON boolean
  * a JSON number
  * a JSON string
  * an array of JSON booleans
  * an array of JSON numbers
  * an array of JSON strings
* "scope" - an OPTIONAL string to distinguish a specific use of the value.
* "links" - as defined in {{!RFC9083}}.

Each object of the array MUST only appear once per combination of "name" and "scope". As
"scope" is optional, an object of the array without a scope MUST only appear once with a
"name".

The "simpleValues_data" object MUST only appear at the top-level of an RDAP object class instance.

The following is an example of an RDAP "ip network" object with a "simpleValues_data" object:

```
{
   "rdapConformance" : [ "rdap_level_0", "simpleValues" ],
   "objectClassName" : "ip network",
   "handle" : "203.0.113.0 - 203.0.113.255",
   "ipVersion" : "v4",
   "startAddress" : "203.0.113.0",
   "endAddress" : "203.0.113.255",
   "simpleValue_data" : [
     {
        "name" : "ip announce auth key pem",
        "value" : [
           "---BEGIN KEY---",
           "349390202000020",
           "---END KEY---"
        ],
        "scope" : "cloud provider alpha"
     },
     {
        "name" : "ip announce auth key pem",
        "value" : [
           "---BEGIN KEY---",
           "abcdefghijklmno",
           "---END KEY---"
        ],
        "scope" : "cloud provider gamma"
     },
   ]
}
```

# Security Considerations

TODO Security


# IANA Considerations

TODO: Registery "simpleValues" RDAP extension identifier.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
