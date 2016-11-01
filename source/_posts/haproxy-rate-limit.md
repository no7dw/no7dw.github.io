title: haproxy rate-limit
date: 2016-11-01 22:28:45
tags:
---
# haproxy rate-limit 

    global
        log /dev/log    local0
        log /dev/log    local1 notice
        maxconn 5
        chroot /var/lib/haproxy
        user haproxy
        group haproxy
        daemon

    defaults
            log     global
            mode    http
            option  httplog
            option  dontlognull
            contimeout 5000
            clitimeout 50000
            srvtimeout 50000
            errorfile 400 /etc/haproxy/errors/400.http
            errorfile 403 /etc/haproxy/errors/403.http
            errorfile 408 /etc/haproxy/errors/408.http
            errorfile 500 /etc/haproxy/errors/500.http
            errorfile 502 /etc/haproxy/errors/502.http
            errorfile 503 /etc/haproxy/errors/503.http
            errorfile 504 /etc/haproxy/errors/504.http
    
    listen admin_stats
            bind *:1081
            mode http
            option httplog
            log 127.0.0.1 local0 err
            maxconn 10
            stats refresh 30s
            stats uri /admin?stats
            stats auth admin:admin
    
    frontend http-in
        bind *:80
        default_backend servers
    
        tcp-request inspect-delay 5s
    
        acl document_request path_beg -i /v2
    #    acl is_upload hdr_beg(Content-Type) -i multipart/form-data
        acl too_many_uploads_by_user sc0_gpc0_rate() gt 10
        acl mark_seen sc0_inc_gpc0 gt 0
    
        stick-table type string size 1k store gpc0_rate(60s)
    
        #tcp-request content track-sc0 hdr(Authorization) if METH_GET document_request
    
        use_backend 429_slow_down if mark_seen too_many_uploads_by_user
    
    backend 429_slow_down
      timeout tarpit 2s
      errorfile 500 /etc/haproxy/errors/429.http
      http-request tarpit
    
    backend servers
        server server1 127.0.0.1:8000 maxconn 2
        
        


------
### config file


### backend server.js


     'use strict';
        
        var express = require('express');
        var i = 0;
        // Constants
        var PORT = 8000;
        
        // App
        var app = express();
        app.get('/v2', function (req, res) {
          console.log("got %d", i++);
          setTimeout( function(){
          res.send('Hello world\n');}, 200);
        
        });
        
        app.listen(PORT);
        console.log('Running on http://localhost:' + PORT);

 

### test shell command

    $for i in {0..1000}; do (curl -Is http://localhost/v2 &)2>/dev/null;done

