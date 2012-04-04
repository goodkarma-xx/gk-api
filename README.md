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
  https://goodkarmaapp.com/api/v1/user/username@yourcompany.com | python -mjson.tool  ```

and would receive

```json
{
    "user": {
        "balance": 5, 
        "contributed": 181, 
        "user_id": "username@yourcompany.com", 
        "wallet": [
            {
                "amount": 5, 
                "expires_at": 1341289935, 
                "fallback_nonprofit": {
                    "id": 1, 
                    "name": "Sean Casey Animal Rescue", 
                    "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                    "url": "https://goodkarmaapp.com/np/seancasey"
                }, 
                "issued_at": 1333513935, 
                "token": "c8cf5dea04a844b884e5ad339e97dcbc"
            }
        ]
    }
}
```

We are currently in beta so if you are interesting in trying this out, feel free to get in touch [here] (mailto:api@thegoodkarma.co) and we will provide you with a key to use for the time being.

Campaigns
---------

>As an api user, we allow you to create campaigns to help raise money for a specific nonprofit as well as set a fundraising goal for your users to reach.

You can create a campaign with a `POST` request to `https://goodkarmaapp.com/api/v1/campaign`






Users
-----


Points
------


Contributions
-------------


IFrame Contributions
---------------------






