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

## HTTP Verbs

Nosdav supports the retrieval and storage of any type of file using the following HTTP verbs:

1. `GET`: The GET verb is used to retrieve a file. The request URI should be in the following format: `/{nostr-id}/{file-path}`. Upon receiving a GET request, the server should respond with the file corresponding to the specified nostr-id and file-path. If the specified file-path does not exist or the nostr-id is invalid, the server should respond with a 404 status code.

2. `PUT`: The PUT verb is used to update an existing file or create a new one. The request URI should be in the following format: `/{nostr-id}/{file-path}`. The request body should contain the updated file. Upon receiving a PUT request, the server should update the existing file or create a new one if it does not exist. If the request body is invalid or the request cannot be completed for some other reason, the server should respond with an appropriate HTTP status code.

## Authentication

To ensure the authenticity of a request, the client should include a header with a signed nostr event. The header should be in the following format: `Authorization: Nostr base64{base64(signed-event)}`.  The event should be kind=27235 with empty content. The signed-event should be a signature of the nostr event using nostr-id and Schnorr signature scheme.

## File Types

NosDAV supports the storage of any type of file including `.json`. The server should use the file extension to determine the file type and respond with the appropriate content type header. For example, if the file extension is `.txt`, the server should respond with the following header: `Content-Type: text/plain`. Similarly, if the file extension is `.html`, the server should respond with the following header: `Content-Type: text/html`. For `.json` files, the server should respond with the following header: `Content-Type: application/json`.

## Identity

NosDAV uses `nostr-id` as an identity to store and retrieve files. The `nostr-id` specified in the request URI identifies the nostr event associated with the files. The server should validate the `nostr-id` to ensure that the request is authorized.

---

This specification provides a flexible framework for storing files using HTTP requests. By supporting any type of file and using `nostr-id` as an identity, it enables clients to store and retrieve files securely. The authentication mechanism ensures the authenticity of requests and the server responds with the appropriate content type header based on the file extension. The use of the Schnorr signature scheme provides an additional layer of security to the authentication process.



## License

- MIT
