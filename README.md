<div align="center">  
  <h1>NosDAV</h1>
</div>

<div align="center">  
<i>spec</i>
</div>

---

<div align="center">
<h4>Documentation</h4>
</div>

---

[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/nosdav/spec/blob/gh-pages/LICENSE)
[![npm](https://img.shields.io/npm/v/nosdav-spec)](https://npmjs.com/package/nosdav-spec)
[![npm](https://img.shields.io/npm/dw/nosdav-spec.svg)](https://npmjs.com/package/nosdav-spec)
[![Github Stars](https://img.shields.io/github/stars/nosdav/spec.svg)](https://github.com/nosdav/spec/)

# NosDAV - Nostr Distributed Authoring and Versioning

NosDAV is a distributed protocol similar to WebDAV, designed to be used in conjunction with the [Nostr](https://github.com/nostr-protocol/nostr) network. It provides a secure and efficient way to store files in the cloud using HTTP requests. The protocol includes a robust authentication mechanism that requires clients to send a signed Nostr event in a custom header to verify the authenticity of requests. By leveraging the power of Nostr and HTTP, NosDAV provides an innovative solution for secure cloud storage

The NosDAV spec is extended with optional [NAVs](https://nosdav.com/navs) which are NosDAV Advancement Visions.

## Identity

Provisioning of identity and new accounts is outside the scope of this specification. However, it is assumed that users are on the Nostr network. A recommended strategy for a NosDAV provider is to use per-user storage in the form of '/nostr-id/', where 'nostr-id' is the Nostr public key

- see also [NAV-01](https://nosdav.com/navs/01.html)

## HTTP Verbs

NosDAV supports the retrieval and storage of any type of file using the following HTTP verbs:

- see also [NAV-02](https://nosdav.com/navs/02.html)

### GET 

1. `GET`: The GET verb is used to retrieve a file. The request URI should be in the following format: `/{nostr-id}/{file-path}`. Upon receiving a GET request, the server should respond with the file corresponding to the specified nostr-id and file-path. If the specified file-path does not exist or the nostr-id is invalid, the server should respond with a 404 status code.

![image](https://user-images.githubusercontent.com/65864/229709609-107e4570-9407-4ebf-9f50-0f745e752eb6.png)

### PUT

2. `PUT`: The PUT verb is used to update an existing file or create a new one. The request URI should be in the following format: `/{nostr-id}/{file-path}`. The request body should contain the updated file. Upon receiving a PUT request, the server should update the existing file or create a new one if it does not exist. If the request body is invalid or the request cannot be completed for some other reason, the server should respond with an appropriate HTTP status code.

![image](https://user-images.githubusercontent.com/65864/229709383-55475e5a-8ee3-4b0a-a177-dd88030089e6.png)


## Authentication Header

To ensure the authenticity of a request, the client should include a header with a signed nostr event. The header should be in the following format: `Authorization: Nostr base64{base64(signed-event)}`.  The event should be kind=27235 with empty content. The signed-event should be a signature of the nostr event using nostr-id and Schnorr signature scheme.
```json
{
  "kind": 27235,
  "created_at": "Math.floor(http://Date.now() / 1000)",
  "tags": [["u", "path"]],
  "content": ""
}
```
The event above should be signed with a public key

The server should check the signature, the freshness of the created_at timestamp and the url tag

- see also [NAV-03](https://nosdav.com/navs/03.html)

## Access Control

By default, access control in NosDAV allows anyone to retrieve a URI using the GET verb. However, authentication is required to modify a URI using the PUT verb.

- see also [NAV-04](https://nosdav.com/navs/04.html)

## CORS

NosDAV servers should include CORS headers to enable cross-origin requests. The following headers should be set:

    Access-Control-Allow-Origin: '*'
    Access-Control-Allow-Methods: GET, PUT, OPTIONS
    Access-Control-Allow-Headers: Content-Type, Authorization"

- see also [NAV-05](https://nosdav.com/navs/05.html)

## Content Types

NosDAV allows for storage of any content type, including .json. The server should determine the file type based on its extension and respond with the appropriate Content-Type header. For example, .txt files should be served with Content-Type: text/plain, .html files with Content-Type: text/html, and .json files with Content-Type: application/json.

- see also [NAV-06](https://nosdav.com/navs/06.html)

## Discovery

Discovery of user storage is not covered fully in this spec, as it will be application speciric.  App specific data MAY be used with kind=30078 and a d tag as specified in , and a `storage` tag indicated the base of the storage URI.

While NosDAV provides a flexible framework for storing files, the specification does not cover the discovery of user storage in full detail. This is because the discovery process is likely to be application-specific. However, the use of custom data with the kind=30078,  and a d tag, as specified in [NIP-78](https://nips.be/78), may be used to provide application-specific information. Additionally, a storage tag may be saved on relays to indicate the preferred storage URI, for each user.

- see also [NAV-07](https://nosdav.com/navs/07.html)

## Summary

This specification provides a flexible framework for storing files using HTTP requests. By supporting any type of file and using `nostr-id` as an identity, it enables clients to store and retrieve files securely. The authentication mechanism ensures the authenticity of requests and the server responds with the appropriate content type header based on the file extension. The use of the Schnorr signature scheme provides an additional layer of security to the authentication process.


## Extensions

Extensions are provided in the [NAVs](https://nosdav.com/navs/).  Some examples could be:

- Private files - only you can see a file
- Shared files - you choose who can see a file
- Paywall - pay to see a file
- Quotas - quota limits determine access to the API


## Implementations

- [JavaScript Server](https://nosdav.com/server/)

## License

- MIT
