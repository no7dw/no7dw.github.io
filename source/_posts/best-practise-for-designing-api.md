title: best practices for designing  web api
date: 2016-11-24 19:21:07
tags:
---
# best practices for designing web api

### restful
why:
 - meaningful
This will be improve efficiency , less documents , just read the code 
 - auto generate support
Resource can be achieve automatically without writing any code  according to the data model or (java) repository 
 - DRY 
It becomes unnecessary to think about what's good web url for providing api to the client developer --- Just following the standard

### related resource
example get the portfolios of 90 days of use id 123 :
>  http://www.example.com/user/123/portfolios/90 

also another :
>  http://www.example.com/apidoc/201607/docid/98

### use standard http status code
standard without extra documents, we should return http status code according to [the standard](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
useful http status code we should know: 
 - 200 OK :  response for GET PUT DELETE request . 
 - 201 Created
 - 304 Not Modified
 - 302 Found : Redirect
 - 400 Bad Request
 - 401 Unauthorized
 - 403 Forbidden : logged in user, but not allow to access the resource
 - 405 Method Not Allowed: usually for POST/PUT/DELETE request but is now request by GET request
 - 429 Too Many Request: for rate limit

**401 403 405 are usually used right**
**when a POST request  post succeed, but no new result generated, we should return 200 , instead of 201**

### security
 - with authentication :
 Many of the request should first logged in to get the access token in order to have the  permission to access resources. Usually  we have several simple ways to carry the token in the request body :
   - within the http header
   - within the cookies 
   - within the url : http://www.example.com/user/123?token=a12dc5
 - with hash
Another way to keep the web request safe is to use hash code the encrypt the web parameters , example:  http://www.example.com/user/123/order/541?amount=100&product=wade&hash=d328af;
hash the parameters : amount=100&product=wade  
**besides , usually the parameters is sorted by alphabet**

To avoid **middle man problem**, we should use https.
 
### standard Oauth 2.0 example

[prosper oauth flow](https://developers.prosper.com/docs/authenticating-with-oauth-2-0/authorization-key-flow/)
simple we should do :
  - register to get client id and client secrect
  - app granted by the user to get a auth_key according to user's grant permission
  - now we can access the api via the access token and refresh token, both of which get an expire time
  - if token has expired , we need to re-access the api

request example:

```
  POST <prosper_base_address>/security/oauth/token
   Accept: application/json
   Content-type: application/x-www-form-urlencoded

  grant_type=refresh_token&client_id=<your_client_id>&client_secret=<your_client_secret>&refresh_token=<existing_refresh_token_from_user_token_request>
```

response example:

```
{
   "access_token": "5098afd7-f216",
   "token_type": "bearer",
   "refresh_token": "7fcb8a8a-e7dd",
   "expires_in": 3599
}
```


Note that:  

### error standard
 should support http status code 、error code 、error msg
 example with a 200 OK:
 

```
   {
     "code": -1,
     "message": "Something gone wrong",
     "description": "optional print your stack message here"

   }
```
### dig into well know company api example
[mouseflow](https://api-docs.mouseflow.com/?shell#update-website-details) 
update a new website 

> PUT /websites/{website-id}

```
curl -X PUT -u my@email.com:token1234 -d '{"name": "myshop2.com", "recordingRate": 2}' https://api-us.mouseflow.com/websites/{website-id}
```
simple [prosper api](https://developers.prosper.com/docs/investor/accounts-api/)

> https://developers.prosper.com/docs/investor/accounts-api/
**Authorization is in header** -- which help us to  which one is operating user

### ref
[best practices for a pragmatic restful api](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
[API 杂谈](http://36kr.com/p/5049025.html)

