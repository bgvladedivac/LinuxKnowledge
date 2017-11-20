## Apache HTTPD
A web server is a daemon that speaks the http(s) protocol, a text-based protocol for sending and receiving objects over a network connection. The http protocol is sent over the wire in clear text using port 80/TCP by default (though ot her ports can be used). There is also a TLS/SSL encrypted version of the protocol called https that uses port 443/TCP by default.

A basic http exchange has the client connecting to the server, and then requesting a resource using the GET command. Other commands like HEAD and POST exist, allowing clients to request just meta data for a resource, or send the server more information. The following is an extract from a short http exchange:

The client starts by requesting a resource (the GET command), and then follows up with some extra headers, telling the server what types of encoding it can accept, what language it would prefer .... 
```{r, engine='bash', count_lines}
GET /hello . html HTTP/1 .1
Host: github.com
User -Agent : Mozilla/5 .0 { Xll ; Linux x86_64 ; rv : 24.0) Gecko/20100101 Firefox/24 .0
Accept : text/html 
Accept - Language : en - US , en ; q=0.5
```

The server then replies with a status code (HTTP/ 1 .1 200 OK ), followed by a list of headers. The Content - Type header is a mandatory one, telling the client what type of content is being sent. After the headers are done, the server sends an empty line, followed by the requested content. The length of this content must match the length ind icated in the Content - Length header. 

```{r, engine='bash', count_lines}
HTTP/1 .1 200 OK
Date : Tue , 28 May 2015 09:57:40 GMT
Server : Apache/2 .4.6 ( Red Hat ) OpenSSL/1 .0.le - fips mod_wsgi/3 .4 Python/2 .7.5
Content-Lengthh : 12
Keep -Alive : timeout=5, max=82
Connect ion : Keep -Alive
Content-Type : text/plain ; charset=UTF - 8
Hello World ! 
```
