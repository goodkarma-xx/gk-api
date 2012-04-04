The brand new Good Karma API
=============================

We're excited to announce v1 of the [Good Karma API](http://goodkarmaapp.com). With only a few HTTP calls, you can let your customers, users, or visitors to your site give to nonprofits of their choice. It's a great way to align your brand with the interests of your users.

Making a request
-----------------

All URLs start with `https://goodkarmaapp.com/api/v1/`

It is a REST-style API that uses JSON for serialization and header based authentication. At this point all requests require authorization.

Authentication
---------------

In order to make an API call you must provide an API key in the authentication header. It should be in the form:

`Authentication: Bearer YOUR_API_KEY`

For example, if you wish to see the state of a current user in your system, you could make a call like this:

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/user/username@yourcompany.com | python -mjson.tool
```

and would receive something like this back

```json
{
    "user": {
        "balance": 5, 
        "contributed": 181, 
        "user_id": "username@yourcompany.com", 
        "wallet": [
            ... , ...
        ]
    } 
}
```

We are currently in beta so if you are interesting in trying this out, feel free to get in touch [here] (mailto:api@thegoodkarma.co) and we will provide you with a key to use for the time being.

Campaigns
---------

>As an api user, we allow you to create campaigns to help raise money for a specific nonprofit as well as set a fundraising goal for your users to reach.

You can create a campaign with a `POST` request to `https://goodkarmaapp.com/api/v1/campaign`

```shell
curl --data '{"goal": 1000, "name": "Our first campaign!","description": "We really care about animals","nonprofit_id": 1 }' \
  --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/campaign | python -mjson.tool
```

```json
{
    "campaign": {
        "active": true, 
        "description": "We really care about animals", 
        "goal": 1000, 
        "id": 20, 
        "name": "Our first campaign!", 
        "nonprofit": {
            "id": 1, 
            "name": "Sean Casey Animal Rescue", 
            "tagline": "Coming to the aid of unfortunate animals of all kinds", 
            "url": "https://goodkarmaapp.com/np/seancasey"
        }, 
        "percent_complete": 0.0, 
        "raised": 0
    }
}
```
You can view all your campaigns with a `GET` request to `https://goodkarmaapp.com/api/v1/campaign`

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" https://goodkarmaapp.com/api/v1/campaign | python -mjson.tool
```

```json
{
    "campaigns": [
        {
            "active": true, 
            "description": "We really care about animals", 
            "goal": 1000, 
            "id": 1, 
            "name": "Our first campaign!", 
            "nonprofit": {
                "id": 1, 
                "name": "Sean Casey Animal Rescue", 
                "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                "url": "https://goodkarmaapp.com/np/seancasey"
            }, 
            "percent_complete": 17.1, 
            "raised": 171
        }, 
	....
}
```

Users
-----

``


Points
------


Contributions
-------------


IFrame Contributions
---------------------






