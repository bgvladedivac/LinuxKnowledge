## Web servers and the need for them
A web server is a daemon that speaks the http(s) protocol, a text-based protocol for sending and receiving objects over a network connection. The http protocol is sent over the wire in clear text using port 80/TCP by default (though ot her ports can be used). There is also a TLS/SSL encrypted version of the protocol called https that uses port 443/TCP by default.

A basic http exchange has the client connecting to the server, and then requesting a resource using the GET command. Other commands like HEAD and POST exist, allowing clients to request just meta data for a resource, or send the server more information. The following is an extract from a short http exchange:

The client starts by requesting a resource (the GET command), and then follows up with some extra headers, telling the server what types of encoding it can accept, what language it would prefer .... 
```{r, engine='bash', count_lines}
GET /hello . html HTTP/1 .1
Host: localhost
Accept : text/html 
Accept - Language : en - US
```

The server then replies with a status code (HTTP/ 1 .1 200 OK ), followed by a list of headers. The *Content - Type* header is a mandatory one, telling the client what type of content is being sent. After the headers are done, the server sends an empty line, followed by the requested content. 

```{r, engine='bash', count_lines}
HTTP/1 .1 200 OK
Date : Tue , 28 May 2015 09:57:40 GMT
Server : Apache/2 .4.6 
Content-Lengthh : 12
Content-Type : text/plain ; charset=UTF - 8
Hello World ! 
```

While the http protocol seems simple at first, implementing all of the protocol-along with security measures, support for clients not adhering fully to the sta ndard, and support for dynamically generated pages-is not an easy task. That is why most application developers do not write their own web servers, but instead write their appl ications to be run behind a web server like Apache HTTPD.

## About Apache httpd
Apache HTTPD, sometimes just called "Apache" or **httpd** implements a fully configurable and extendable web server with full http support. The functionality of httpd can be extended with *modules*, small pieces of code that plug into the main web server framework and extend its functionality.

Once **httpd-manual** is installed, and the **httpd.service** service is started, the full Apache HTTPD manual is available on http://localhost/manual.  A default dependency of the httpd package is the *httpd-tools* package. This package includes tools to manipulate password maps and databases, tools to resolve IP addresses in logfiles to hostnames ... 

## Basic configuration
A default configuration is written to */etc/httpd/conf/httpd.conf* . This configuration serves out the contents of */var/www/html* for requests coming into any hostname over plain http. The basic syntax of the httpd.conf is comprised of two parts: *Key Value configuration directives*, and HTML-like *<Blockname parameter> *blocks with other configuration directives embedded in them.
  
```{r, engine='bash', count_lines}
 [root@localhost bluetooth]# grep -i serverroot /etc/httpd/conf/httpd.conf
# ServerRoot: The top of the directory tree under which the server's
# ServerRoot at a non-local disk, be sure to specify a local disk on the
# same ServerRoot for multiple httpd daemons, you will need to change at
ServerRoot "/etc/httpd"
```
This ServerRoot will specify where httpd will look for any files referenced in the configuration files with a relative path name. 

```{r, engine='bash', count_lines}
[root@localhost bluetooth]# grep -i listen /etc/httpd/conf/httpd.conf
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to
#Listen 12.34.56.78:80
Listen 80
```
This directive tells httpd to start listening on port 80/TCP on all interfaces.  


```{r, engine='bash', count_lines}
[root@localhost bluetooth]# grep -i include /etc/httpd/conf/httpd.conf
Include conf.modules.d/*.conf
IncludeOptional conf.d/*.conf

[root@localhost bluetooth]# grep -i documentroot /etc/httpd/conf/httpd.conf
DocumentRoot "/var/www/html"
```
