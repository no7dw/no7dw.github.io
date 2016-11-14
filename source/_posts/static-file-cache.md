title: static file cache
date: 2016-11-14 22:13:07
tags:
---
# static file cache

------

### not request but from disk 

set chrome network: 
from disk

how to implement it:

set header Cache-Control max-age


    app.use(function (req, res, next) {
      if (! ('JSONResponce' in res) ) {
        return next();
      }
    
      res.setHeader('Cache-Control', 'public, max-age=31557600');
      res.json(res.JSONResponce);
    })


### not change
http status code 304
how to impletment it?


### cache it 
  - nginx 
  - varnish 
  - cdn

### deploy problem
when js/css/html change:
 
    http://example.com/static/xxx.js?v=1.0.0
    http://example.com/static/yyy.css?v=1.0.0

#### problem:
    need to modify version manually
    sovle it by use contents verison(something like crc32 or md5):
    
    - http://example.com/static/yyy.css?v=af321
    - http://example.com/static/yyy.css?v=4239330460
    
### deploy order 
    Gated Launch
    
refer:
    - http://www.cnblogs.com/lyzg/p/5125934.html
    - https://www.zhihu.com/question/20790576
    
    


