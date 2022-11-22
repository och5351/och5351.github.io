---
title: "Java KeyStore"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true

date: 2022-11-17
last_modified_at: 2022-11-17

---

<br><br>

# 1. Java KeyStore

<br>

암호 기능의 두 가지 핵심 요소는 암호 알고리즘과 암호키이다. JCA/CJE API를 통해 생성된 암호키는 Java가 제공하는 KeyStore에 저장되도록 되어 있다.

지원하는 KeyStore Type은 JKS, JCEKS, PKCS12 이다.

* JKS : Java KeyStore
* JCEKS : Java Cryptography Extensions KeyStore
* PKCS12 : Public Key Cryptography Standards

JKS, JCEKS, PKCS12 파일 양식 모두, password-based encryption(PBE) 방식을 사용한다. 즉 사용자가 입력한 비밀번호(password)를 가지고, 암호키를 파생(derive)하고, 파생된 암호키로 Private-key(또는 Secret-key)를 암호화 시켜 keystore 파일 안에 저장한다.

JKS 파일의 확장자는 “.jks” 이며, JCEKS 파일의 확장자는 “.jck” 이며, PKCS12의 확장자는 “.p12” 이다.

<br><br>

# 2. keytool 활용 keystore 생성

<br>

```sh
keytool

# Key and Certificate Management Tool

# Commands:

#  -certreq            Generates a certificate request
#  -changealias        Changes an entry's alias
#  -delete             Deletes an entry
#  -exportcert         Exports certificate
#  -genkeypair         Generates a key pair
#  -genseckey          Generates a secret key
#  -gencert            Generates certificate from a certificate request
#  -importcert         Imports a certificate or a certificate chain
#  -importpass         Imports a password
#  -importkeystore     Imports one or all entries from another keystore
#  -keypasswd          Changes the key password of an entry
#  -list               Lists entries in a keystore
#  -printcert          Prints the content of a certificate
#  -printcertreq       Prints the content of a certificate request
#  -printcrl           Prints the content of a CRL file
#  -storepasswd        Changes the store password of a keystore

# Use "keytool -command_name -help" for usage of command_name
```

```sh
keytool -genkeypair -keystore my_key -storetype jceks 
```

```sh
keytool -list -keystore my_key.jck
```



<br><br>