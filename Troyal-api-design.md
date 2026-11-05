Base URL: `https://api.troyal.example/v1`
Authentication: API key sent in the request header.

## Works


| Resource | Method | Path | Description | Auth | Result |
--- | --- | --- | --- | --- | ---
| Works | GET | /works | Lists all works in your account | API Key | 200 OK |
| Works | POST | /works | Creates a new work | API Key | 201 Created |
| Works | GET | /works/{id} | Retrieves one work | API Key | 200 Ok |
| Works | PATCH | /works/{id} | Updates a work | API Key | 200 OK |
| Works | DELETE | /works/{id} | Deletes a work | API Key | 204 No Content |



## Ownership splits

| Resource | Method | Path | Description | Auth | Result |
--- | --- | --- | --- | --- | ---
| Ownership splits | GET |  /works/{id}/splits  | Lists the ownership splits for a work | API Key | 200 OK |
| Ownership splits | POST |  /works/{id}/splits | Adds an owner and their percentage to a work | API Key | 201 Created |
| Ownership splits | PATCH | works/{id}/splits/{split_id} | Updates an owner's percentage | API Key | 200 Ok |
| Ownership splits | DELETE | works/{id}/splits/{split_id} | Removes an owner from a work | API Key | 204 No Content |


## Royalty Statements 

| Resource | Method | Path | Description | Auth | Result |
--- | --- | --- | --- | --- | ---
| Royalty Statements | GET |  /statements  | Lists all royalty statements | API Key | 200 OK |
| Royalty Statements | GET | /statements/{id} | Retrieves one statement | API Key | 200 OK |
| Royalty Statements | POST | /statements | Generates a statement for a payee and period | API Key | 201 Created |




