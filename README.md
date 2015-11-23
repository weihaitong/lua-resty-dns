Name
====

#support edns-client-subnet
lua-resty-dns quote "https://github.com/openresty/lua-resty-dns"
lua-resty-dns - Lua DNS resolver for the ngx_lua based on the cosocket API. support edns-client-subnet.

Synopsis
========

```lua
    lua_package_path "/path/to/lua-resty-dns/lib/?.lua;;";

    server {
        location = /dns {
            content_by_lua '
                local resolver = require "resty.dns.resolver"
                local r, err = resolver:new{
                    nameservers = {"8.8.8.8", {"8.8.4.4", 53} },
                    retrans = 5,  -- 5 retransmissions on receive timeout
                    timeout = 2000,  -- 2 sec
                }

                if not r then
                    ngx.say("failed to instantiate the resolver: ", err)
                    return
                end

       <modify> local answers, err = r:query("www.google.com")
		local answers, err = r:query("www.google.com", {client = client_ip})
                if not answers then
                    ngx.say("failed to query the DNS server: ", err)
                    return
                end

                if answers.errcode then
                    ngx.say("server returned error code: ", answers.errcode,
                            ": ", answers.errstr)
                end

                for i, ans in ipairs(answers) do
                    ngx.say(ans.name, " ", ans.address or ans.cname,
                            " type:", ans.type, " class:", ans.class,
                            " ttl:", ans.ttl)
                end
            ';
        }
    }
``` 
see Also
========
* https://github.com/openresty/lua-resty-dns
* http://noops.me/?p=653

[Back to TOC](#table-of-contents)
