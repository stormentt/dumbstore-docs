# POST /register

HTTP Basic Auth w/ Blank Body

| Code | Reason |
| ---- | ------ |
| 201  | user created |
| 403  | registration not allowed |
| 409  | username already exists |
| 500  | internal server error |

# POST /change-password

HTTP Basic Auth w/ new password in body

| Code | Reason |
| ---- | ------ |
| 200  | password changed |
| 401  | bad username or password |
| 500  | internal server error |

# POST /store

HTTP Basic Auth w/ Blank Body

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

# POST /store/{transfer-id}

HTTP Basic Auth w/ Blob in Body

Headers should contain Offset: and Length:

Offset will be assumed to be 0

| Code | Reason |
| ---- | ------ |
| 200  | upload received |
| 400  | bad request |
| 401  | bad username or password |
| 404  | bad transfer id or no access | 
| 500  | internal server error |

# GET /store/{transfer-id}

HTTP Basic Auth w/ Blank Body

On success will return

```
{
  "expected-size": 
  "received-size":
  "status":
}
```

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

# DELETE /store/{transfer-id}

HTTP Basic Auth w/ Blank Body

Aborts the transfer

| Code | Reason |
| ---- | ------ |
| 200 | transfer aborted, or transfer didn't exist in the first place, or no access to the transfer |
| 401 | bad username or password |
| 500 | internal server error |

# GET /blob/{hash}

HTTP Basic Auth w/ Blank Body

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

# HEAD /blob/{hash}

HTTP Basic Auth w/ Blank Body

Response header will include Length

| Code | Reason |
| ---- | ------ |
| 200  | blob exists & access allowed | 
| 400  | bad request |
| 401  | bad username or password |
| 404  | blob not found or no access ;) |
| 500  | internal server error |

# DELETE /blob/{hash}

HTTP Basic Auth w/ Blank Body

Deletes the blob

| Code | Reason |
| ---- | ------ |
| 200  | blob deleted, or didn't exist, or no access |
| 401  | bad username or password |
| 500  | internal server error |

# POST /alias/

HTTP Basic Auth w/ Blank Body

Creates a new alias, pointing to nothing, and returns the id

| Code | Reason |
| ---- | ------ | 
| 201  | alias created |
| 401  | bad username or password |
| 500  | internal server error |


# POST /alias/{id}

HTTP Basic Auth w/ new hash in body

Updates the alias to point at the blob hash

| Code | Reason |
| ---- | ------ |
| 200  | alias updated |
| 400  | bad blob hash format |
| 401  | bad username or password |
| 404  | alias not found or no access |
| 500  | internal server error |

# GET /alias/{id}

HTTP Basic Auth w/ Blank Body

Gets the current hash for the alias

| Code | Reason |
| ---- | ------ |
| 200  | success |
| 401  | bad username or password |
| 404  | alias not found or no access|
| 500  | internal server error |

# DELETE /alias/{id}
HTTP Basic Auth w/ Blank Body

Deletes the alias

| Code | Reason |
| ---- | ------- |
| 200  | alias deleted, alias didn't exist, or no access |
| 401  | bad username or password |
| 500  | internal server error |
