# users

| column  | description |
| ------- | ----------- |
| id       | user id         |
| username | username        |
| salt     | prehash salt    |
| password | hashed password |

# blobs

| column | description |
| ------ | ----------- |
| hash   | blob's hash      |
| owner  | id of owner      |
| size   | size of blob     |
| path   | filepath of blob |

# transfers

| column | description |
| ------ | ----------- |
| id          | transfer id              |
| owner       | id of owner              |
| size        | bytes uploaded so far    |
| final\_size | expected size of blob    |
| status      | status of transfer       |
| expires     | transfer expiration date |


# aliases

| column | description |
| ------ | ----------- |
| id     | alias ID    |
| owner  | id of owner |
| hash   | blob's hash |
