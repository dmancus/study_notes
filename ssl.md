SSL Handshake:
==============
```
Client says "hello"
```
```
    Server responds "hello" back to client
    Server chooses key, cypher, and hash method
    Server sends certificate to client with the PUBLIC key
```
```
Client validates certificate and public key
Client generates symmetric key and sends it to server (encrypted with server's public key so only server can decrypt that)
```
```
    SSL session established, both sides are now using symmetric encryption
```


Chain of Trust:
===============
* Server has its own ssl certificate  
* Server needs to be verified by an intermedicate certificate authority (ICA), ICA vouches for server's ssl cert  
* ICA in turn is vouched for by root certificate authority  
* Trust cascades down from root to ICA to server certificate  
* Client side must have root CAs installed so they know which root CAs are trustworthy...only chains that go up to one of your installed root CAs will be trusted on your system
