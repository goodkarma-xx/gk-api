The brand new Good Karma API
=============================

We're excited to announce v1 of the [Good Karma API](http://goodkarmaapp.com). With only a few HTTP calls, you can let your customers, users, or visitors to your site give to nonprofits of their choice. It's a simple way to align your brand with the interests of your users as well as having tax deductible benefits.

If you are interested in trying it out, please contact us [here] (mailto:api@thegoodkarma.co) and we will hook you up with an api key.

Making a request
-----------------

All URLs start with `https://goodkarmaapp.com/api/v1/`

It is a REST-style API that uses JSON for serialization and header based authentication. At this point all requests require authorization and are scoped to your organization/api key.

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

`GET` request to `https://goodkarmaapp.com/api/v1/user/:user_id` will return info on the username

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/user/test_user_id
```

Would return:

```json
{
    "user": {
        "balance": 5, 
        "contributed": 181,
        "user_id": "test_user_id", 
        "wallet": [
            {
                "amount": 5, 
                "expires_at": "2012-07-04T02:33:29", 
                "fallback_nonprofit": {
                    "id": 1, 
                    "name": "Sean Casey Animal Rescue", 
                    "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                    "url": "https://goodkarmaapp.com/np/seancasey"
                }, 
                "issued_at": "2012-04-05T02:33:29", 
                "token": "c8tf5dha04a844b884e5ad339e97dcbc"
            }
        ]
    }
}
```

Note: You don't have to worry about creating a user in our system the first time you make a query for a user -- our backend was designed to be transparent to your actual users in that regard.

Points
------

>Now for the fun part, awarding users points! As an api user, you can distribute points from your balance to users or visitors.

`POST` to `https://goodkarmaapp.com/api/v1/points/reward`

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
    --data '{"amount": 4, "fallback_nonprofit_id": 1, "user_id": "d@c.com"}' \
    https://goodkarmaapp.com/api/v1/points/reward | python -mjson.tool
```

If you have enough points remaining, the call would return 

```json
{
    "points": {
        "amount": 4, 
        "expires_at": "2012-07-05T02:33:29", 
        "fallback_nonprofit": {
            "id": 1, 
            "name": "Sean Casey Animal Rescue", 
            "tagline": "Coming to the aid of unfortunate animals of all kinds", 
            "url": "https://goodkarmaapp.com/np/seancasey"
        }, 
        "issued_at": "2012-04-05T02:33:29", 
        "token": "a924bef1ci4a43eeh3576bfed77ba3e4"
    }
}
```


If you do not have enough points in your account you will receive an error like this:

```json
{
    "error": "Insufficient funds. You tried to reward 4 but you have a balance of 0"
}
```

Although we included a `user_id` value in this call, you do not have to associate points with a specific user. For example, if you want to reward a visitor to your site, you can just make this call, and then redeem the points using the `token` field.

Contributions
-------------

`POST` to `https://goodkarmaapp.com/api/v1/contribution`

You can redeem all of a user's points like so:

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
  --data '{"user_id": "d@c.com", "campaign_id": 1, "tokens": "all"}' \
  https://goodkarmaapp.com/api/v1/contribution
```

or you can redeem specific tokens 

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
    -data '{"user_id": "d@c.com", "tokens": ["23f23r23r23r","3423fw2f24"]}' \
    https://goodkarmaapp.com/api/v1/contribution
```

Which would return

```json
{
    "contribution": {
        "amount": 4, 
        "campaign": {
            "active": true, 
            "description": "We really care about animals", 
            "goal": 1000, 
            "id": 1, 
            "name": "Barkbox 1", 
            "nonprofit": {
                "id": 1, 
                "name": "Sean Casey Animal Rescue", 
                "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                "url": "https://goodkarmaapp.com/np/seancasey"
            }, 
            "percent_complete": 19.0, 
            "raised": 190
        }, 
        "made_at": "2012-04-05T02:33:29", 
        "user_id": "d@c.com"
    }
}
```

The `user_id` is not compulsory in this example if the `token` you are using was not rewarded to a `user_id` in the first place.

IFrame Contributions
---------------------

>Not everyone will want to provide their own contribution UI so we also provide an API endpoint to generate an iframe that you can embed into your own site. It provides a simple interface where users can decide which of your campaigns they want to contribute towards.

You can either create an IFrame on the contribution level or on the user level. This means you can authenticate an Iframe for a single contribution (using a contribution token), or for all of the unused points a user has collected (using their user id).

A contribution level IFrame is useful for one off rewards, where as a user_id level IFrame is more useful for a user page or profile.

`POST` to `https://goodkarmaapp.com/api/v1/iframe`

* User level authentication

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" --data '{"user_id": "username"}' \
   https://goodkarmaapp.com/api/v1/iframe | python -mjson.tool
```
* Contribution level authentication

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" --data '{"token": "CONTRIBUTION_TOKEN"}'
  https://goodkarmaapp.com/api/v1/iframe | python -mjson.tool
```

Will return something like this:

```json
{
    "iframe_url": "https://goodkarmaapp.com/api/v1/iframe/0d9ea50i46did4019bi8a12845d682642"
}
```

You can customize the background of the iframe by appending a `bg` parameter to the end of the url and the hex value you wish to color it with. Eg. `?bg=ccc` or `?bg=8FC5DF`

We also provide a simple js utility script to allow you to place the the iframe into your site

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8">
		<script src="https://goodkarmaapp.com/static/js/iframe.goodkarma.js" type="text/javascript"></script>
	</head>
	<body>
		<div id="gkframe"></div>
		<script type="text/javascript">
			GoodKarma.nonProfitSelector("gkframe","https://goodkarmaapp.com/api/v1/iframe/0d9ea50i46did4019bi8a12845d682642")
		</script>
	</body>
</html>
```
