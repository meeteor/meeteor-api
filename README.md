Meeteor API
==========

This is a REST-style API that uses JSON for serialization and and authentication token for authentication.

Making a request
----------------

All URLs start with `https://api.meeteor.com/v1/`. **SSL only**. The path is prefixed with the API version. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

To make a request for all the matches for a user, you'd append `matches` to the users API base url to form something like https://api.meeteor.com/v1/users/124/matches. In curl, that looks like:

```shell
curl https://api.meeteor.com/v1/users/124/matches?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd
```

Authentication
--------------

Authentication is handled through an `AUTH_TOKEN` query string parameter. This token will be assigned by Meeteor to your organization. We are happy to reissue tokens as much as you like. This token properly identifies your organization and allows use to isolate your data.

Somes examples using the `AUTH_TOKEN`:

```shell
curl https://api.meeteor.com/v1/users/124/matches?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd
curl https://api.meeteor.com/v1/users/124?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd
```

Application Flow
----------------

1. You send us user data as user's signup/edit their profile. You can send us any attributes you find interesting and would like us to consider during the matching process.

2. In your application, make a request to the `matches` API to get matches for a particular user.


Users
=====

Create user
-----------

* `POST /users` will create a new user from the parameters passed.

Sample endpoint:

`https://api.meeteor.com/v1/users?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd`

```json
{
  ID: 123,
  location: “New York”,
  fb_connections: [1234, 5678 ]
  education: [
    {
      school: “University of Pennsylvania”,
      major: Finance,
      degree: MBA,
      grad_year: 2011
    },
    {
      school: “University of Pennsylvania”,
      major: Finance,
      degree: MBA
    }
  ],
  work: [
    {
      employer: “Apple”,
      position: “Product Manager”,
      description: “Details about role and responsibilities”
    }
  ],
  skills: [“Entrepreneurship”, “Sales”, “SEO”, “Marketing”],
  interests: [“Venture Capital”, “Startups”]
}

```
This will return `201 Created`, with the current JSON representation of the user if the creation was a success. If the application does not have access to create new users, you'll see `403 Forbidden`.

Update user
-----------

* `PUT /users/:id` will update the user from the parameters passed.

Sample endpoint:

`https://api.meeteor.com/v1/users/123?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd`

```json
{
  skills: [“Ruby”, “PHP”, “MySQL”]
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the user. If the application does not have access to update the user, you'll see `403 Forbidden`.


Delete user
-----------

* `DELETE /users/:id`

Sample endpoint:

`https://api.meeteor.com/v1/users/123?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd`

This will delete the user specified and return `204 No Content` if that was successful. If the application does not have access to delete the user, you'll see `403 Forbidden`.

Matches
-------

* `GET /users/:id/matches` will retrieve matches for a user.

Sample endpoint:

`https://api.meeteor.com/v1/users/123/matches?AUTH_TOKEN=2opi3hsadfkh23p4oi234asfd`

```json
{
  user_id: 123
  matches: [
    {
      user_id: 12
      why: ["Work at Google", "Went to Virginia Tech"]
    },
    {
      user_id: 584
      why: ["Also interested in Marketing"]
    }
  ]
}
```

This will return the JSON representation of the matches shown above. If the application does not have access to update the user, you'll see `403 Forbidden`.
