# jenkins-oidc-discovery

Public mirror of `jenkins.unbounce.net`'s OIDC discovery documents.

Jenkins itself is only reachable via VPN, so external services that need to
verify a Jenkins-issued OIDC id token (starting with Doppler, for PS-4934)
can't reach Jenkins' own `/oidc/.well-known/openid-configuration` and
`/oidc/jwks` endpoints directly. Jenkins' OpenID Connect Provider plugin
supports pointing at an alternate, publicly reachable issuer URL instead,
this repo is that location.

Jenkins still signs every token with its own private key. These two files
are a public mirror of its discovery document and public key only, nothing
here is a secret. Per the plugin's own warning, though: unauthorized
modification of either file could allow tokens to be forged, so changes
here should go through normal PR review, not a direct push.

## Files

- `.well-known/openid-configuration`, the discovery document, `issuer` and
  `jwks_uri` rewritten to point at this repo's GitHub Pages URL instead of
  Jenkins' own internal one.
- `jwks.json`, Jenkins' public signing key, copied as-is.

## Keeping this in sync

If Jenkins' OIDC signing key ever rotates, `jwks.json` here needs updating
to match, or token verification will start failing for every service that
trusts this issuer. There's no automation for this yet, it's a manual step.
