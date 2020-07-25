# Project Overview
Dumbstore is a dumb storage program with authentication.  
It receives blobs of data over HTTP2 and stores them, providing an identifier to retrieve them later.

# Authentication
Authentication happens with HTTP Basic Auth

# Storage
Incoming blobs are hashed with Blake2-256 with username as key, which is used to index them.  
The blobs themselves are stored by default in `/var/dumbstore/data` but that can be changed by config.  
Subfolders are created by XXXX/XXXX/[full hash].  
The metadata for the blob (size, filepath, owner, etc) is stored in a postgres server, indexed by hash.  

# Aliases
Blobs can be aliased by a numeric ID, to allow for special updating data

# Operations
Dumbstore can store, retrieve, and delete data, using the identifier & Basic Auth.

# Initialisms:
| Initialism | Meaning |
| ---------- | ------- |
| UKB-Hash   | Username-Keyed Blake2b Hash |

# Motivations
| Thing | Why |
| ----- | --- |
| Blake2-256 keyed with Username | Fast, easy to implement, prevents two users from having duplicate ID's |
| HTTP Basic Auth | Simplicity, nearly every HTTP2 client has support |
