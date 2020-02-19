---
title: Retry-Scope header field
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

informative:

--- abstract

This document defines
the Retry-Scope header field for HTTP
thus allowing a server to communicate the scope
of the returned Retry-After header field.

--- note_Note_to_Readers

*RFC EDITOR: please remove this section before publication*

Discussion of this draft takes place on the HTTP working group mailing list
(ietf-http-wg@w3.org), which is archived at
<https://lists.w3.org/Archives/Public/ietf-http-wg/>.

The source code and issues list for this draft can be found at
<https://github.com/ioggstream/draft-polli-Retry-Scope>.


--- middle

# Introduction

The Retry-After header {{!SEMANTICS=RFC7231}}, Section 7.1.3 allows a server
to indicate how long the user agent ought to wait before making a follow-up request.

While Retry-After applies to the issued request, it may be useful for the server
to communicate to the user agent that the conditions that lead to returning Retry-After
are broader in scope than a single request.

This proposal allows a server to convey that scope
in the Retry-Scope response header field,
and ask the client to temporarily refrain
from making other requests to the same resource,
or even to all resources on the same server.


## Notational Conventions
{::boilerplate bcp14+}

This document uses the Augmented BNF defined in {{!RFC5234}} and updated
by {{!RFC7405}} along with the "#rule" extension defined in Section 7 of
{{!MESSAGING=RFC7230}}
and the URI-reference rule defined in Section 2.7 of {{!MESSAGING}}. 

The terms "intermediaries" and "target URI" are to be interpreted as described in {{!MESSAGING}}.

# Header Specifications

The following header is defined

## Retry-Scope {#retry-scope-header}

The Retry-Scope response header field indicates that
the conditions that lead to returning Retry-After
are broader in scope than a single request.

~~~ abnf
   Retry-Scope = URI-reference
~~~

Two examples of Retry-Scope:

~~~
   Retry-Scope: /books
   Retry-Scope: https://api.example/
~~~

A user agent receiving the Retry-Scope header field
in conjunction with a Retry-After header field
ought to wait before making further request
to the resource identified by the Retry-Scope field value.

This header MUST NOT be repeated;
if a user agent receives multiple Retry-Scope header fields,
then it SHOULD ignore them.

Intermediaries aware of the Retry-Scope semantics
(eg. reverse proxies)
MAY modify the Retry-Scope
in order to help the user agent to correctly identify the scope
and ensure that the field value matches the target URI,
like they would have done
for the Location header field defined in Section 7.1.2 of {{SEMANTICS}}.

# Security Considerations

## Role of intermediaries

An intermediary, by chance or purpose,
might alter the scope of the Retry-Scope
thus causing the user agent
to refrain contacting other server resource.

When the server originating
the Retry-Scope is behind one or more intermediaries
it is possible that the field value is not consistent
with the target URI.

# IANA Considerations

## Retry-Scope Header Field Registration

This section registers the `Retry-Scope` header field in the "Permanent Message
Header Field Names" registry ({{!RFC3864}}).

Header field name:  `Retry-Scope`

Applicable protocol:  http

Status:  standard

Author/Change controller:  IETF

Specification document(s):  {{retry-scope-header}} of this document

--- back

# FAQ
{: numbered="false"}

Q: Why not using link relations?
:  This solution is simpler and was previously discussed
   [here](https://github.com/httpwg/http-core/pull/317#issuecomment-585868767).

# Acknowledgements

This specification was born from a thread created by Martin Thomson,
and the subsequent discussion.

# Change Log
{: numbered="false"}

RFC EDITOR PLEASE DELETE THIS SECTION.

