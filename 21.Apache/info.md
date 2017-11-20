## Apache HTTPD
A web server is a daemon that speaks the http(s) protocol, a text-based protocol for sending and receiving objects over a network connection. The http protocol is sent over the wire in clear text using port 80/TCP by default (though ot her ports can be used). There is also a TLS/SSL encrypted version of the protocol called https that uses port 443/TCP by default.

A basic http exchange has the client connecting to the server, and then requesting a resource using the GET command. Other commands like HEAD and POST exist, allowing clients to request just meta data for a resource, or send the server more information. The following is an extract from a short http exchange:

```{r, engine='bash', count_lines}
GET /hello . html HTTP/1 .1
Host: github.com
User -Agent : Mozilla/5 .0 { Xll ; Linux x86_64 ; rv : 24.0) Gecko/20100101 Firefox/24 .0
Accept : text/html 
Accept - Language : en - US , en ; q=0.5
```
