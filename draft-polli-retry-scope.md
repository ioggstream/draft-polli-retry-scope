---
title: Retry scope
abbrev:
docname: draft-polli-retry-scope-latest
category: std

ipr: trust200902
area: General
workgroup:
keyword: Internet-Draft

stand_alone: yes
pi: [toc, tocindent, sortrefs, symrefs, strict, compact, comments, inline, docmapping]

author:
 -
    ins: R. Polli
    name: Roberto Polli
    org: Team Digitale, Italian Government
    email: robipolli@gmail.com

normative:
  UNIX:
    title: The Single UNIX Specification, Version 2 - 6 Vol Set for UNIX 98
    author:
      name: The Open Group
      ins: The Open Group
    date: 1997-02

informative:
  RFC2818:
  RFC5788:
  RFC6962:
  RFC7396:

--- abstract

This document defines
the Retry-Scope header field for HTTP
thus allowing a server returning a Retry-After header field
to communicate the scope for that header field. 

--- note_Note_to_Readers

*RFC EDITOR: please remove this section before publication*

Discussion of this draft takes place on the HTTP working group mailing list
(ietf-http-wg@w3.org), which is archived at
<https://lists.w3.org/Archives/Public/ietf-http-wg/>.

The source code and issues list for this draft can be found at
<https://github.com/ioggstream/draft-polli-Retry-Scope>.


--- middle

# Introduction

The Retry-After header {{RFC7231}}, Section 7.1.3 allows a server
to indicate how long the user agent ought to wait before making a follow-up request.

While Retry-After applies to the issued request, it may be useful for the server
to communicate to the user agent that the conditions that lead to returning Retry-After
or other service management headers,
are broader in scope than a single request.

## This proposal

This proposal allows a server to include the above information using the Retry-Scope header field,
and ask to the client to temporarily refrain
from making other requests to the same resource,
or even to all resources on the same server


## Notational Conventions
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 ([RFC2119] and [RFC8174])
when, and only when, they appear in all capitals, as shown here.

This document uses the Augmented BNF defined in {{!RFC5234}} and updated
by {{!RFC7405}} along with the "#rule" extension defined in Section 7 of
{{!RFC7230}}.


# Header Specifications

The following header is defined

## Retry-Scope {#Retry-Scope-header}

The Retry-Scope response header field indicates the application
scope of Retry-After.

    Retry-Scope = "Retry-Scope" ":" OWS quoted-string

Other header fields, 
MAY use this header field to convey their scope.

Two examples of Retry-Scope:

~~~
   Retry-Scope: "/books"
   Retry-Scope: "https://api.example/"
~~~


# Security Considerations

## Role of intermediaries

TBD

# IANA Considerations

## Retry-Scope Header Field Registration

This section registers the `Retry-Scope` header field in the "Permanent Message
Header Field Names" registry ({{!RFC3864}}).

Header field name:  `Retry-Scope`

Applicable protocol:  http

Status:  standard

Author/Change controller:  IETF

Specification document(s):  {{Retry-Scope-header}} of this document


# Change Log

RFC EDITOR PLEASE DELETE THIS SECTION.


# Acknowledgements

This specification was born from a thread created by Martin Thomson,
and the subsequent discussion 

# FAQ

  Q: Why not using link relations
  :  This solution is simpler and was previously discussed [here](https://github.com/httpwg/http-core/pull/317#issuecomment-585868767) 