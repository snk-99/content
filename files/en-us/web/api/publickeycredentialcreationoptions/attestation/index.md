---
title: PublicKeyCredentialCreationOptions.attestation
slug: Web/API/PublicKeyCredentialCreationOptions/attestation
tags:
- API
- Property
- PublicKeyCredentialCreationOptions
- Reference
- Web Authentication API
- WebAuthn
browser-compat: api.PublicKeyCredentialCreationOptions.attestation
---
<p>{{APIRef("Web Authentication API")}}{{securecontext_header}}</p>

<p><strong><code>attestation</code></strong> is an optional property of the
  {{domxref("PublicKeyCredentialCreationOptions")}} dictionary. This is a string whose
  value indicates the preference regarding the attestation transport, between the
  authenticator, the client and the relying party.</p>

<p>The attestation is a mean for the relying party to verify the origin of the
  authenticator with an attestation certificate authority. The information contained in
  the attestation may thus disclose some information about the user (e.g. which device
  they are using).</p>

<h2 id="Syntax">Syntax</h2>

<pre
  class="brush: js"><em>attestation </em>= <em>publicKeyCredentialCreationOptions</em>.attestation</pre>

<h3 id="Value">Value</h3>

<p>A string which may be</p>

<ul>
  <li><code>"none"</code>: the relying party is not interested in this attestation. This
    avoids making a check with the attestation certificate authority and asking the user
    consent for sharing identifying information.</li>
  <li><code>"indirect"</code>: the client may change the assertion from the authenticator
    (for instance, using an <a
      href="https://w3c.github.io/webauthn/#anonymization-ca">anonymization CA</a>). This
    value is used when the relying party wishes to verify the attestation.</li>
  <li><code>"direct"</code>: the relying party wants to receive the attestation as
    generated by the authenticator.</li>
</ul>

<p>The default value is <code>"none"</code>.</p>

<h2 id="Examples">Examples</h2>

<pre class="brush: js">var publicKey = {
  attestation: "indirect",
  challenge: new Uint8Array(26) /* this actually is given from the server */,
  rp: {
    name: "Example CORP",
    id  : "login.example.com"
  },
  user: {
    id: new Uint8Array(26), /* To be changed for each user */
    name: "jdoe@example.com",
    displayName: "John Doe",
  },
  pubKeyCredParams: [
    {
      type: "public-key",
      alg: -7
    }
  ]
};

navigator.credentials.create({ publicKey })
  .then(function (newCredentialInfo) {
    // send attestation response and client extensions
    // to the server to proceed with the registration
    // of the credential
  }).catch(function (err) {
     console.error(err);
  });</pre>

<h2 id="Specifications">Specifications</h2>

{{Specifications}}

<h2 id="Browser_compatibility">Browser compatibility</h2>

<p>{{Compat}}</p>

<h2 id="See_also">See also</h2>

<ul>
  <li>{{domxref("AuthenticatorAttestationResponse.attestationObject")}}</li>
</ul>