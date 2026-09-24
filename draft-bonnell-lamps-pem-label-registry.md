---
title: "An IANA Registry for PEM Labels"
abbrev: "PEM Label Registry"
category: std

docname: draft-bonnell-lamps-pem-label-registry-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Limited Additional Mechanisms for PKIX and SMIME"
keyword:
 - PEM
 - textual encoding
 - IANA registry
venue:
  group: "Limited Additional Mechanisms for PKIX and SMIME"
  type: "Working Group"
  mail: "spasm@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/spasm/"
  github: "CBonnell/draft-bonnell-pem-label-registry"
  latest: "https://CBonnell.github.io/draft-bonnell-pem-label-registry/draft-bonnell-lamps-pem-label-registry.html"

author:
 -
    fullname: Corey Bonnell
    organization: TurboLight Solutions, LLC
    email: corey.bonnell@turbolightsolutions.com
 -  fullname: Rob Stradling
    organization: Sectigo Ltd.
    email: rob@sectigo.com

normative:
  RFC6960:
  RFC7468:
  RFC8126:

informative:
  RFC2315:
  RFC2986:
  RFC4648:
  RFC4716:
  RFC5208:
  RFC5280:
  RFC5652:
  RFC5755:
  RFC5915:
  RFC5958:
  RFC8823:
  RFC7848:
  RFC9361:
  RFC9580:
  RFC9934:

--- abstract

RFC 7468 describes the textual encodings, commonly known as "PEM", of
several PKIX, PKCS, and CMS structures, each identified by a label such as
`CERTIFICATE`. No registry of these labels exists, which has led to
discoverability issues and inconsistent use. This document establishes an IANA registry
for PEM labels and defines the `OCSP RESPONSE` label.


--- middle

# Introduction

{{RFC7468}} defines the textual encoding of cryptographic structures in which
base64-encoded data is placed between encapsulation boundaries of the form
`-----BEGIN label-----` and `-----END label-----`. The label identifies the
format of the data carried in the base64 text.

{{RFC7468}} specifies a fixed set of labels, and other specifications have
since defined or used additional labels. Without a registry, implementers have
no authoritative place to find which labels are in use or which document
defines the format of the encapsulated data, and specification authors have no
way to avoid collisions.

This document creates the "PEM Labels" registry and populates it with
labels previously defined in various documents. It also defines a label for OCSP
responses ({{ocsp-response}}). It does not change the textual encoding
itself.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Textual Encoding of OCSP Responses {#ocsp-response}

OCSP responses {{RFC6960}} are encoded using the `OCSP RESPONSE` label. The
encoded data MUST be a DER-encoded OCSPResponse as described in
{{Section 4.2.1 of RFC6960}}.

Generators MUST use the `OCSP RESPONSE` label when encoding an OCSP response.
All other rules for generators and parsers in {{RFC7468}} apply.


# Security Considerations

This document only creates a registry and introduces no new security
considerations. The security considerations of {{RFC7468}} continue to
apply.

Registration of a label does not imply that the format it identifies, or any
algorithm used with that format, is secure or recommended. The label is an unauthenticated hint, and the encapsulated data needs to
be validated according to the specification of its format regardless of the
label.


# IANA Considerations

IANA is requested to create a new registry titled "PEM Labels". The
registration procedure is Specification Required {{RFC8126}}.

Each registration contains the following fields:

Label:
: The label that appears in the encapsulation boundaries. It MUST conform to
  the `label` ABNF production in {{Section 3 of RFC7468}}.

Format Reference:
: The document that specifies the format of the data carried in the base64 text.

Reference:
: The document that defines the label and its use with the textual encoding.

The initial contents of the registry are:

| Label                 | Format Reference          | Reference   |
|:----------------------|:--------------------------|:------------|
| CERTIFICATE           | {{RFC5280}}               | {{RFC7468}} |
| X509 CRL              | {{RFC5280}}               | {{RFC7468}} |
| CERTIFICATE REQUEST   | {{RFC2986}}               | {{RFC7468}} |
| PKCS7                 | {{RFC2315}}               | {{RFC7468}} |
| CMS                   | {{RFC5652}}               | {{RFC7468}} |
| PRIVATE KEY           | {{RFC5208}}, {{RFC5958}}  | {{RFC7468}} |
| ENCRYPTED PRIVATE KEY | {{RFC5958}}               | {{RFC7468}} |
| ATTRIBUTE CERTIFICATE | {{RFC5755}}               | {{RFC7468}} |
| PUBLIC KEY            | {{RFC5280}}               | {{RFC7468}} |
| EC PRIVATE KEY        | {{RFC5915}}               | {{RFC5915}} |
| PGP MESSAGE           | {{RFC9580}}               | {{RFC9580}} |
| PGP PUBLIC KEY BLOCK  | {{RFC9580}}               | {{RFC9580}} |
| PGP PRIVATE KEY BLOCK | {{RFC9580}}               | {{RFC9580}} |
| PGP SIGNATURE         | {{RFC9580}}               | {{RFC9580}} |
| ECHCONFIG             | {{RFC9934}}               | {{RFC9934}} |
| ENCODED SMD           | {{RFC7848}}               | {{RFC9361}} |
| OCSP RESPONSE         | {{RFC6960}}               | RFC XXXX    |
{: title="Initial PEM Labels Registry Contents"}

RFC Editor: please replace "RFC XXXX" with the RFC number assigned to this
document and remove this note.

## Guidance for Designated Experts

The designated experts are expected to verify that:

* The label conforms to the syntax in {{Section 3 of RFC7468}} and does not
  differ from an existing registration only in case or whitespace.

* The use of the label conforms to the encapsulation boundaries and base64
  encoding defined in {{Section 3 of RFC7468}}, rather than a similar but
  distinct textual convention. For example, the `SSH2 PUBLIC KEY` label
  defined by {{RFC4716}} uses an encapsulation boundary with a different
  number of dashes than {{RFC7468}}, and the `ACME RESPONSE` label defined
  by {{RFC8823}} encloses base64url-encoded data rather than the base64
  alphabet specified in {{Section 4 of RFC4648}}; neither is a suitable
  candidate for registration as currently specified.

* The referenced specification clearly identifies the format of the
  encapsulated data (for example, the ASN.1 type and its encoding).

* A new label is warranted, rather than reuse of an existing label whose
  format already accommodates the data.

The experts may also approve registrations of labels already in widespread
use, provided a stable specification describing the format is available.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
