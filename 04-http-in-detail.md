## What is HTTP? (HyperText Transfer Protocol)

HTTP is what's used whenever you view a website, developed by Tim Berners-Lee and his team between 1989-1991. HTTP is the set of rules used for communicating with web servers for the transmitting of webpage data, whether that is HTML, Images, Videos, etc.

## What is HTTPS? (HyperText Transfer Protocol Secure)

HTTPS is the secure version of HTTP. HTTPS data is encrypted so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that you're talking to the correct web server and not something impersonating it.

## What is a URL? (Uniform Resource Locator)

If you’ve used the internet, you’ve used a URL before. A URL is predominantly an instruction on how to access a resource on the internet.
>http://user:password@website.com80/user?id=1#task2

- - - Example Request:
GET / HTTP/1.1

Host: website.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://website.com/

- - - Example Response:
HTTP/1.1 200 OK

Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98


<html>
<head>
    <title>websitename</title>
</head>
<body>
    Welcome To website.com
</body>
</html>

## Http Methods:
HTTP methods are a way for the client to show their intended action when making an HTTP request. There are a lot of HTTP methods but we'll cover the most common ones, although mostly you'll deal with the GET and POST method.

>>>GET Request

This is used for getting information from a web server.

>>>POST Request

This is used for submitting data to the web server and potentially creating new records

>>>PUT Request

This is used for submitting data to a web server to update information

>>>DELETE Request

This is used for deleting information/records from a web server.

## HTTP Status Codes:

100-199 - Information Response	These are sent to tell the client the first part of their request has been accepted and they should continue sending the rest of their request. These codes are no longer very common.
------------------------------------------------
200-299 - Success	This range of status codes is used to tell the client their request was successful.
------------------------------------------------
300-399 - Redirection	These are used to redirect the client's request to another resource. This can be either to a different webpage or a different website altogether.
------------------------------------------------
400-499 - Client Errors	Used to inform the client that there was an error with their request.
------------------------------------------------
500-599 - Server Errors	This is reserved for errors happening on the server-side and usually indicate quite a major problem with the server handling the request.

## Cookies :

Cookies can be used for many purposes but are most commonly used for website authentication. The cookie value won't usually be a clear-text string where you can see the password, but a token (unique secret code that isn't easily humanly guessable).
## Pics:

![alt text](http3.png) ![alt text](http1.png) ![alt text](http2.png)
