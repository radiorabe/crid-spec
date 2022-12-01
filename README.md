# RaBe Content Reference Identifier Specification

The RaBe Content Reference Idenitifier Spcification (CRID) defines how we use
the `crid:` URL scheme as defined by [ETSI TS 102 822-4](https://tech.ebu.ch/docs/metadata/ts_10282204v010501p.pdf)
for CRIDs that use a dns portion delegated to RaBe.

## Table of Contents

- [Overview](#overview)
- [Notations and Terminology](#notations-and-terminology)
- [Versioning](#versioning)
- [Authority](#authority)
- [CRID](#crid)
- [Example](#example)

## Overview

The TV-Anytime content reference described in ETSI TS 102 82204 aims at allowing acquisition of a specific instance of a specific item of content.
To acheive this, it provides the possibility to refer to content independent of it's location.
It also describes how such a reference can be resolved into one or more locations for obtaining the content.

RaBe does not plan on fully adopting and implementing the full ETSI TS 102 82204 stack.
Specifically we do not plan to implement an actual authority and it's corresponding XML interfaces.

This specification defines how we structure the data part of a CRID for the `rabe.ch` domain name.

## Notations and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## Versioning

This specification is versioned using git tag. It uses [go-semantic-release](https://go-semantic-release.xyz/)
for tagging new versions following the [Conventional Commits v1.0.0 specification](https://www.conventionalcommits.org/en/v1.0.0/).

## Authority

The TV-anytime specification uses DNS to provide unique names for each authority, ours are these:

* `rabe.ch`

The RaBe CRID authority is responsible for creating unambiguous CRIDs.

The RaBe CRID authority SHALL NOT provide centralized CRID resolution services.
We rely on involved parties to resolve RaBe CRIDs on their own.

## CRID

RaBe CRIDs are as simple as possible:

```text
crid://rabe/<version>/<data-content>#t=clock:19961108T143720.25Z
```

### `<version>` data part

### `<data-content>` data part

### ABNF definition

```abnf
crid          =   "crid://rabe.ch/" version "/" data-content
version       =   "v" 1*DIGIT [ pre-release ]          ; ie. v1, v2,
pre-release   =   ( "alpha" / "beta" / "rc" ) *DIGIT   ; v1alpha, v1alpha1, v1beta, ...

data-content  =   show-name [ "#" media-frags ]
show-name     =   1*ALPHA     ; show name string derived from website
media-frags   =   utc-range   ; based on https://www.w3.org/TR/media-frags/

utc-range     =   "t=clock:" utc-date-time [ "-" utc-date-time ]
utc-date-time =   utc-date "T" utc-time "Z"
utc-date      =   8DIGIT                    ; < YYYYMMDD >
utc-time      =   6DIGIT [ "." fraction ]   ; < HHMMSS.fraction >
fraction      =   1*2DIGIT                  ; 0-99
```

## Example

This section is non-normative.

| Show | Description | Web URL | CRID |
| ---- | ---- | ---- | ---- |
| Der Morgen (Freitag) | morning show, is a different show for each day of the week, does not have repeats and no individual episodes on the website | `https://rabe.ch/der-morgen-freitag/` | `crid://rabe.ch/v1/der-morgen-freitag` |
| RaBe Info | news, different on each day of the week, gets repeated once on air and published as podcast, has a page per episode on the web site | `https://rabe.ch/info/`  | `crid://rabe.ch/v1/info` |
| Klangbecken | Always on show, gets used as a filler if nothing is scheduled and has it's own schedule, no repeats, no episodes | `https://rabe.ch/klangbecken/`  | `crid://rabe.ch/v1/klangbecken` |
| Klangbecken | A specific point-in-time of the Klanbecken show, usually the start of a new track we play | `https://rabe.ch/klangbecken/ ` | crid://rabe.ch/v1/klangbecken#t=clock=20211201T131200.00Z` |

## Links

This section is non-normative.

* [RFC 4078](https://datatracker.ietf.org/doc/html/rfc4078)

## License

This specification is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Copyright

Copyright (c) 2021 [Radio Bern RaBe](http://www.rabe.ch)
