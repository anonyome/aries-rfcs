# Aries RFC 0880: SD-JWT signed W3C Credential (vc+sd-jwt) Attachment format for requesting and issuing credentials

- Authors: George Mulhearn (Anonyome Labs)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2025-12-10
- Supersedes:
- Start Date: 2025-12-10
- Tags: [feature](/tags.md#feature), [protocol](/tags.md#protocol), [credentials](/tags.md#credentials)

## Summary

This RFC registers an attachment format for use in the [issue-credential V2](../0453-issue-credential-v2/README.md) protocol based on W3C credentials with [SD-JWT signatures](https://www.w3.org/TR/vc-jose-cose/#with-sd-jwt) from the [VC Data Model 2.0](https://www.w3.org/TR/vc-data-model-2.0/).

It defines a minimal set of parameters needed to create a common understanding of the verifiable credential to issue. It is based on version [2.0 of the Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model-2.0/) which is a W3C recommendation since 15 May 2025.

## Motivation

The Issue Credential protocol needs an attachment format to be able to exchange W3C credentials with SD-JWT signatures.

## Tutorial

Complete examples of messages are provided in the [reference section](#reference).

## Reference

### `vc+sd-jwt-detail` attachment format

Format identifier: `didcomm/vc+sd-jwt-detail@v1.0`

This format is used to formally propose, offer, or request a credential. The `credential` property should contain the credential as it is going to be issued, without any `proof` or `credentialStatus` properties. `options` is reserved for future usage.

The JSON structure might look like this:

```json
{
  "credential": {
    "@context": [
      "https://www.w3.org/ns/credentials/v2",
      "https://www.w3.org/ns/credentials/examples/v2"
    ],
    "id": "urn:uuid:3978344f-8596-4c3a-a978-8fcaba3903c5",
    "type": ["VerifiableCredential", "UniversityDegreeCredential"],
    "issuer": "did:key:z6MkodKV3mnjQQMB9jhMZtKD9Sm75ajiYq51JDLuRSPZTXrr",
    "validFrom": "2020-01-01T19:23:24Z",
    "validUntil": "2021-01-01T19:23:24Z",
    "credentialSubject": {
      "id": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH",
      "degree": {
        "type": "BachelorDegree",
        "name": "Bachelor of Science and Arts"
      }
    }
  },
  "options": {
  }
}
```

A complete [`request credential` message form the Issue Credential protocol 2.0](../0453-issue-credential-v2/README.md#request-credential) might look like this:

```jsonc
{
  "@id": "7293daf0-ed47-4295-8cc4-5beb513e500f",
  "@type": "https://didcomm.org/issue-credential/%VER/request-credential",
  "comment": "<some comment>",
  "formats": [
    {
      "attach_id": "13a3f100-38ce-4e96-96b4-ea8f30250df9",
      "format": "didcomm/vc+sd-jwt-detail@v1.0"
    }
  ],
  "requests~attach": [
    {
      "@id": "13a3f100-38ce-4e96-96b4-ea8f30250df9",
      "mime-type": "application/json",
      "data": {
        "base64": "ewogICJjcmVkZW50aWFsIjogewogICAgIkBjb250...(clipped)...IkVkMjU1MTlTaWduYXR1cmUyMDE4IgogIH0KfQ=="
      }
    }
  ]
}
```

- `credential` - Required. Detail of the W3C Credential that will be issued. Properties MUST align with the [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model) (1.1 or 2.0). This also means all properties required by the data model MUST be present. The properties listed below are formally supported, but additional properties MAY be included if it conforms with the data model.

  - `@context`
  - `id`
  - `type`
  - `issuer`
  - `issuanceDate` / `validFrom`
  - `expirationDate` / `validUntil`
  - `credentialSubject`

- `options` - Required. Options for specifying how the VC is created/signed.


### `vc+sd-jwt` attachment format

Format identifier: `didcomm/vc+sd-jwt@v1.0`

This format is used to transmit a verifiable credential with SD-JWT securing mechanism (vc+sd-jwt). The contents of the attachment is an SD-JWT.

The JSON structure might look like this:

```json
"ey...(clipped)...~"
```

A complete [`issue-credential` message from the Issue Credential protocol 2.0](../0453-issue-credential-v2/README.md#issue-credential) might look like this:

```json
{
  "@id": "284d3996-ba85-45d9-964b-9fd5805517b6",
  "@type": "https://didcomm.org/issue-credential/%VER/issue-credential",
  "comment": "<some comment>",
  "formats": [
    {
      "attach_id": "5b38af88-d36f-4f77-bb7a-2f04ab806eb8",
      "format": "didcomm/vc+sd-jwt@v1.0"
    }
  ],
  "credentials~attach": [
    {
      "@id": "5b38af88-d36f-4f77-bb7a-2f04ab806eb8",
      "mime-type": "application/ld+json",
      "data": {
        "json": "ey...(clipped)...~"
      }
    }
  ]
}
```

## Drawbacks

N/A

## Rationale and alternatives

- The `hlindy-zkp-v1.0` format is an alternative restricted to the Hyperledger Indy network. The `dif/credential-manifest@v1.0` allows to issue JSON-LD credentials but is not ready yet for usage.

## Prior art

N/A

## Unresolved questions

N/A

## Implementations

The following lists the implementations (if any) of this RFC. Please do a pull request to add your implementation. If the implementation is open source, include a link to the repo or to the implementation within the repo. Please be consistent in the "Name" field so that a mechanical processing of the RFCs can generate a list of all RFCs supported by an Aries implementation.

| Name / Link | Implementation Notes |
| ----------- | -------------------- |
|             |
