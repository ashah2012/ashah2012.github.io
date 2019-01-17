---
layout: post
title:  "Writing a file in Java to Remote Windows Drive using Credentials"
date:   2019-01-1y 00:00:00 +0530
categories: java
author: "abhishek shah"
tags: java i/o credentials
---

So, a very tiring day at work. I wanted to write a file to a shared network drive. I was given the required credentails to logon to the network drive.
But the question was how do I do it programmtically? 

## jCIFS - The answer

I had never heard about [jCIFS](https://www.jcifs.org/) before today. And I am glad I was able to somehowe come across the right question on stackoverflow.

Coming back to the problem :
* I had to write a file in a remote network drive.
* I was given the required credentials - username, password, and domain.

## Code - solution

The code is fairly simple.

```java 
NtlmPasswordAuthentication auth = new NtlmPasswordAuthentication("domain", "username", "password");
SmbFileInputStream in = new SmbFileInputStream("smb://host/c/My Documents/somefile.txt", auth);
byte[] b = new byte[8192];
int n;
while(( n = in.read( b )) > 0 ) {
    System.out.write( b, 0, n );
}
``` 

As mentioned, in their [API documentation](https://www.jcifs.org/src/docs/api/) -
"the SmbFile, SmbFileInputStream , and SmbFileOutputStream classes are analogous to the File, FileInputStream, and FileOutputStream classes"



