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

NosDAV is a specification that extends WebDAV to provide a way to store files using HTTP requests. The specification includes an authentication mechanism that involves sending a header with a signed nostr event to ensure the authenticity of the request.

## New account provision

Provisioning of new accounts is outside the scope of this specification. However, it is assumed that users are on the Nostr network. A recommended strategy for a NosDAV provider is to use per-user storage in the form of '/nostr-id/', where 'nostr-id' is the Nostr public key

## HTTP Verbs

NosDAV supports the retrieval and storage of any type of file using the following HTTP verbs:

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
  "tags": [["url", "path"]],
  "content": ""
}
```
The event above should be signed with a public key

The server should check the signature, the freshness of the created_at timestamp and the url tag

## Access Control

By default, access control in NosDAV allows anyone to retrieve a URI using the GET verb. However, authentication is required to modify a URI using the PUT verb.

## File Types

NosDAV supports the storage of any type of file including `.json`. The server should use the file extension to determine the file type and respond with the appropriate content type header. For example, if the file extension is `.txt`, the server should respond with the following header: `Content-Type: text/plain`. Similarly, if the file extension is `.html`, the server should respond with the following header: `Content-Type: text/html`. For `.json` files, the server should respond with the following header: `Content-Type: application/json`.

## CORS

NosDAV servers should include CORS headers to enable cross-origin requests. The following headers should be set:

    Access-Control-Allow-Origin: '*'
    Access-Control-Allow-Methods: GET, PUT, OPTIONS
    Access-Control-Allow-Headers: Content-Type, Authorization"

## Discovery

Discovery of user storage is not covered fully in this spec, as it will be application speciric.  App specific data MAY be used with kind=30087 and a d tag as specified in [NIP-78](https://nips.be/78), and a `storage` tag indicated the base of the storage URI.


## Summary

This specification provides a flexible framework for storing files using HTTP requests. By supporting any type of file and using `nostr-id` as an identity, it enables clients to store and retrieve files securely. The authentication mechanism ensures the authenticity of requests and the server responds with the appropriate content type header based on the file extension. The use of the Schnorr signature scheme provides an additional layer of security to the authentication process.


## Extensions

- Private files - only you can see a file
- Shared files - you choose who can see a file
- Paywall - pay to see a file
- Quotas - quota limits determine access to the API


## Implementations

- [JavaScript Server](https://nosdav.com/server/)

## License

- MIT
