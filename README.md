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

## JSON params

Some endpoints accept JSON params. These are sent as a GET parameter named `json_params`. 
The following options are available (not all are available for all endpoints):
```json
{
  "filters": {
    "used": true,
    "user": {
      "uid": "7163205870"
    },
    "climb": {
      "gym_id": 1337,
      "deleted": false,
      "live": true
    }
  },
  "includes": [
    {
      "gym": [
        "holds",
        "walls",
        "setters"
      ]
    },
    "setters"
  ]
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

`json_params` <kbd>json</kbd> _requires [authentication](#authentication)_  
Example: 
```json
{
  "includes": [
    {
      "gym": [
        "holds",
        "walls",
        "setters"
      ]
    },
    "setters"
  ]
}
```

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

## Ranked gym users

<kbd>GET</kbd> `gyms/{gym}/ranked_gym_users`  
Returns the ranked gym users for a gym.

[Example](https://hopp.sh/r/0AtgTZ5QFcK6)

### Path parameters

`gym` <kbd>int</kbd>  **required**  
The gym to get ranked gym users for.

### Query parameters

`climbs_type` <kbd>string</kbd> **required**  
The type of climbs to get stats for.  
Options: `boulders`, `routes`

`ranking_type` <kbd>string</kbd> **required**  
The type of ranking to get.  
Options: `grade`, `count`

`items_per_page` <kbd>int</kbd>  
The number of items per page.

## Opinions

<kbd>GET</kbd> `opinions`  
Returns user opinions (ratings).

[Example](https://hopp.sh/r/H69vRYMLw6Sf)

### Query parameters

`json_params` <kbd>json</kbd>  
Example: `{"filters":{"used":true,"user":{"uid":"7163205870"},"climb":{"gym_id":8,"deleted":false,"live":true}}}`

## Ascends

<kbd>GET</kbd> `ascends`
Returns ascends.

[Example](https://hopp.sh/r/A6ERCeGcv9Bh)

### Query parameters

`json_params` <kbd>json</kbd> **partially required**  
Example: 
```json
{
  "filters": {
    "used": true,
    "user": {
      "uid": "7163205870" // required
    },
    "climb": {
      "gym_id": 8,
      "deleted": false,
      "live": true
    }
  }
}
```

`serialize_checks` <kbd>boolean</kbd>  
Adds a `checks` field to each ascend. 2 checks is a flash, 1 check is a redpoint. 
Settings this to `false` does not seem to have any effect.