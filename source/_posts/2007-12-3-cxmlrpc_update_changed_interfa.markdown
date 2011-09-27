--- 
layout: post
title: "CXML-RPC update: changed interface, includes server part"
mt_id: 90
date: 2007-12-03 22:45:16 -08:00
---
I didn't really like the DWIM interface for type translations on the client, so I made the type information mandatory. (-:

This means that instead of:

    * (xrpc:call "http://localhost:8080/RPC2" "addNumbers" '(1 2 3.0)) ; WRONG

You get to do this:

    * (xrpc:call "http://localhost:8080/RPC2" "addNumbers" '(:integer 1
	                                                         :double 2
	                                                         :double 3.0))
	6.0
	
This doesn't look vastly superior, but consider this case:

    * (xrpc:call "http://localhost:8080/RPC2" "weekDay" `(:time ,(get-universal-time)))
    0

Where before, you had to construct an xml-rpc-date structure; ew. This also allows nicer handling of (unsigned-byte 8) vectors (you can base64-encode them now), and you don't have to construct yucky xml-rpc-struct objects anymore: just use alists. The decoding functions now return the type tag that they saw in the response or request.

Speaking of requests, the server is now finished, lifting CXML-RPC to more or less the same level as s-xml-rpc. It implements the introspection functions specified in [xmlrpc-c's documentation](http://xmlrpc-c.sourceforge.net/introspection.html), and the [faults_interop spec](http://xmlrpc-epi.sourceforge.net/specs/rfc.fault_codes.php). You can define methods for different handlers, which means that you can attach more than one handler to different URLs and they can expose different sets of methods to the world. This might be handy for those running virtual hosts.

In response to yesterday's post, Rafal Strzalinski pointed me at Ivan Boldyrev's [s-xml-rpc-hunchentoot](http://mesemb.ru/soft/lisp/), which adapts the s-xml-rpc server functions for use with hunchentoot's handlers. There's no port to DRAKMA for the client part. 

I think you should give CXML-RPC a try if you either want to expose multiple sets of methods on different handlers, or you don't want to install aserve just for an xml-rpc client. (-:
