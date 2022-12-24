# TopLogger API docs
Documentation for the undocumented TopLogger API.

Endpoint: https://api.toplogger.nu/v1

## Authentication

Some endpoints can return extra data that is only accessible to the authenticated user. 
To authenticate, you need to send two headers with your request:
`X-USER-EMAIL` and `X-USER-TOKEN`. These are the email and token of the user you want to authenticate as.
You can find your token by logging in the TopLogger site and inspecting the headers of an authenticated request.

You can also get a token by posting to `users/sign_in`. The body should be a JSON object with the following format:
```json
{
  "user": {
    "email": "youremail@example.com",
    "password": "correct-horse-battery-staple"
  }
}
```

## User

<kbd>GET</kbd> `users/{user}`  
Get a user's information. Returns extra data if authenticated as the user.

[Example](https://hopp.sh/r/TFh1cLpht8JD)

### Path parameters

`user` <kbd>int</kbd>  **required**  
The user to get stats for.

### Query parameters

`json_params` <kbd>json</kbd> _requires authentication_  
Example: `{"includes":[{"gym":["holds","walls","setters"]},"setters"]}`

## User stats

<kbd>GET</kbd> `users/{user}/stats`  
Returns the stats for a user. Included stats are 60d grade & top ten ascends.

[Example](https://hopp.sh/r/gVINK5eL3dmw)

### Path parameters

`user` <kbd>int</kbd>  **required**  
The user to get stats for.

### Query parameters

`gym_id` <kbd>int</kbd>  
The gym to get stats for. Optional but can be very slow without it.

`climbs_type` <kbd>string</kbd>  
The type of climbs to get stats for.  
Options: `boulders`, `routes`

## Strength history

<kbd>GET</kbd> `users/{user}/strength_history`  
Returns the strength history for a user.

[Example](https://hopp.sh/r/q4ugNG5FvJj5)

### Path parameters

`user` <kbd>int</kbd>  **required**  
The user to get stats for.

### Query parameters

`climb_type` <kbd>string</kbd>  
The type of climbs to get stats for.  
Options: `boulders`, `routes`

`offset` <kbd>int</kbd>  
Unknown.

## New & old

<kbd>GET</kbd> `users/{gym}/new_old`  
Returns new and old climbs for a gym.

[Example](https://hopp.sh/r/e0VEZ3mTMNbJ)

### Path parameters

`gym` <kbd>int</kbd>  **required**  
The gym to get new/old climbs for.

### Query parameters

`climbs_type` <kbd>string</kbd>  
The type of climbs to get stats for.  
Options: `boulders`, `routes`
