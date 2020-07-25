# /register
POST: http basic auth with a blank body

| Code | Reason |
| ---- | ------ |
| 201  | user created |
| 403  | registration not allowed |
| 409  | username already exists |
| 500  | internal server error |

# /change-pw
POST: http basic auth with new password in body

| Code | Reason |
| ---- | ------ |
| 200  | password changed |
| 401  | bad username or password |
| 500  | internal server error |

# /store
POST: http basic auth
Headers should contain Length: in bytes
Header may include Hash: "Username-Keyed Blake2b Hash"

On success, will a random transfer-id for completing the transfer
{
  "transfer-id": "[A-Za-z0-9]{16}"
}

| Code | Reason |
| ---- | ------ |
| 200  | resource already exists |
| 201  | auth succeeded & server ready for transfer |
| 401  | bad username or password |
| 500  | internal server error |

# /store/{transfer-id}
POST: http basic auth with blob as body
Headers should contain Offset: and Length:
Offset will be assumed to be 0

| Code | Reason |
| ---- | ------ |
| 200  | upload received |
| 400  | bad request |
| 401  | bad username or password |
| 404  | bad transfer id or no access | 
| 500  | internal server error |

# /store/{transfer-id}
GET: http basic auth

On success will return
{
  "expected-size": 
  "received-size":
  "status":
}

| Transfer Status | Explanation |
| ------ | ----------- |
| uploading | client upload is not complete |
| waiting | upload complete, waiting for processing |
| processing | hashing, validating, storing |
| validation-failed | bad hash |
| stored | completed processing & stored |

| Code | Reason |
| ---- | ------ |
| 200  | nothing has failed so far |
| 201  | processing and storing complete! |
| 400  | processing failed |
| 401  | bad username or password |
| 404  | invalid transfer id or no access |
| 500  | internal server error |

# /store/{transfer-id}
DELETE: http basic auth
Aborts the transfer

| Code | Reason |
| ---- | ------ |
| 200 | transfer aborted, or transfer didn't exist in the first place, or no access to the transfer |
| 401 | bad username or password |
| 500 | internal server error |

# /blob/{hash}
GET: http basic auth
Headers may contain Offset:, assumed 0

Response header will include Length
Response body will be the blob content

| Code | Reason |
| ---- | ------ |
| 200  | success | 
| 400  | bad request |
| 401  | bad username or password |
| 404  | blob not found or no access ;) |
| 500  | internal server error |

# /blob/{hash}
HEAD: http basic auth

Response header will include Length

| Code | Reason |
| ---- | ------ |
| 200  | blob exists & access allowed | 
| 400  | bad request |
| 401  | bad username or password |
| 404  | blob not found or no access ;) |
| 500  | internal server error |

# /blob/{hash}
DELETE: http basic auth
Deletes the blob

| Code | Reason |
| ---- | ------ |
| 200  | blob deleted, or didn't exist, or no access |
| 401  | bad username or password |
| 500  | internal server error |

# /alias/
POST: http basic auth
Creates a new alias, pointing to nothing, and returns the id

| Code | Reason |
| ---- | ------ | 
| 201  | alias created |
| 401  | bad username or password |
| 500  | internal server error |


# /alias/{id}
POST: http basic auth
Body: a blob hash

Updates the alias to point at the blob hash

| Code | Reason |
| ---- | ------ |
| 200  | alias updated |
| 400  | bad blob hash format |
| 401  | bad username or password |
| 404  | alias not found or no access |
| 500  | internal server error |

# /alias/{id}
GET: http basic auth

Gets the current hash for the alias

| Code | Reason |
| ---- | ------ |
| 200  | success |
| 401  | bad username or password |
| 404  | alias not found or no access|
| 500  | internal server error |

# /alias/{id}
DELETE: http basic auth

Deletes the alias
| Code | Reason |
| ---- | ------- |
| 200  | alias deleted, alias didn't exist, or no access |
| 401  | bad username or password |
| 500  | internal server error |

