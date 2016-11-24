title: best practise for designing api
date: 2016-11-24 19:21:07
tags:
---
# best practise for designing api

### restful
why:
 - meaningful
 - auto generate support
 - auto test support
 - DRY 

### repeatable
request many time should response the same

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
 - with authentication
 - with token
 - with hash

### safety
prevent DOS/DDOS , limit speed

### logging
 - support debug
 - support statistics 

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

### ref
[best practices for a pragmatic restful api](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
[API 杂谈](http://36kr.com/p/5049025.html)
