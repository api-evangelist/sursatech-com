# auth.md

SursaTech accepts external AI agents for the public A2A API.

## Audience

Agents can use this flow to call the SursaTech AI Advisor at `https://api.sursatech.com/api/a2a`.
The API exposes company knowledge for SursaTech, an AI-native product
engineering company in Kathmandu, Nepal, including AI agent development, RAG,
product engineering, requirement capture, tentative estimates, and guarded
consultation booking turns. Payment, calendar, cancellation, and rescheduling
rules still run through the normal backend workflow.

## Registration

Supported registration type: `anonymous`. SursaTech does not currently accept
ID-JAG identity assertions or a user-claimed service-auth ceremony for this
public A2A surface.

Register an agent with:

```http
POST https://api.sursatech.com/api/a2a/register
Content-Type: application/json

{"name": "Your agent name"}
```

The response includes `tokenType: Bearer` and `accessToken`. Store the token
securely; the plaintext token is returned only once.

Machine-readable agent registration metadata is published in the
`agent_auth` block of the authorization server metadata. It includes:

- `register_uri`: `https://api.sursatech.com/api/a2a/register`
- `claim_uri`: `https://api.sursatech.com/api/a2a/register`
- `identity_types_supported`: `anonymous`
- `credential_types_supported`: `access_token`, `bearer`

OAuth-style discovery metadata is available at:
https://api.sursatech.com/.well-known/oauth-authorization-server

Protected resource metadata is available at:
https://api.sursatech.com/.well-known/oauth-protected-resource

## Credential Use

Send the token on A2A message calls:

```http
POST https://api.sursatech.com/api/a2a
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Supported scopes: `a2a, company.read, requirements.write, booking.write`. Current self-registration
tokens receive the A2A agent scope and are rate-limited per client and per IP.

## Revocation

There is no self-service revocation endpoint yet. Contact `info@sursatech.com`
to disable a registered agent credential.
