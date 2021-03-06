# http-01 Challenge

With the `http-01` challenge, you prove to the CA that you are able to control the web site content of the domain to be authorized, by making a file with a signed content available at a given path.

`Http01Challenge` provides two strings:

```java
Http01Challenge challenge = auth.findChallenge(Http01Challenge.TYPE);

String token = challenge.getToken();
String content = challenge.getAuthorization();
```

`token` is the name of the file that will be requested by the CA server. It must contain the `content` string, without any leading or trailing white spaces or line breaks. The `Content-Type` header must be either `text/plain` or absent.

The expected path is (assuming that `${domain}` is your domain and `${token}` is the token):

```
http://${domain}/.well-known/acme-challenge/${token}
```

The challenge is completed when the CA was able to download that file and found `content` in it.

Note that the request is sent to port 80 only. There is no way to choose a different port, for security reasons. This is a limitation of the ACME protocol, not of _acme4j_.

## Preferred Address

If your domain name resolves to multiple IP adresses, you can set an explicit address that the CA server should prefer to send the request to. This address must be included in the set of your domain's IP addresses.

```java
Http01Challenge challenge = auth.findChallenge(Http01Challenge.TYPE);
challenge.setAddress(InetAddress.getByName("198.51.100.12"));
```

The server _should_ connect to this address, but is not required to do so.
