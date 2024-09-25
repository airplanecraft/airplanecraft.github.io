+++
Categories = ["Technology"]
Description = ""
Tags = ["jwt","token"]
date = "2021-12-03T10:30:37+08:00"
menu = "token"
title = "JWT"
toc = true
+++

### what is jwt

JSON Web Tokens are an open, industry standard RFC 7519 method for representing claims securely between two parties.

### jwt structure

A well-formed JWT consists of three concatenated Base64url-encoded strings, separated by dots (.):

JOSE Header: contains metadata about the type of token and the cryptographic algorithms used to secure its contents.

JWS payload (set of claims): contains verifiable security statements, such as the identity of the user and the permissions they are allowed.

JWS signature: used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you must check its signature before storing and using it.


### A JWT typically looks like this:

![jwt sample](/images/jwtsample.png)

![jwt diagram](/images/jwt.jpeg)