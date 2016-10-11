# C# & PHP AES Encryption
C#-PHP AES encryption is a project which provides a PHP and a C# class with an AES implementation that is compatible between one another.

# Overview
You are provided with 2 classes, one in C# and one in PHP. Both classes implement an ECB mode AES encryption with each class'es 
output being compatible for decryption by the other one. This means, you can encrypt a block of text using the C# class
and decrypt it on your server with the PHP class and vice-versa. 

# Usage and Tips
You can use these classes if you have a C# application and would like to have an encrypted communication between your client 
and your server. Both classes ensure that the outputs will be compatible for decryption with each other's methods. Be warned however,
that an encrypted communication with a shared key (symmetric encryption) is not enough to protect your sensitive data as it can be
cracked with a MITM attack or by decompiling your C# application and retrieving the encryption key. Your best bet is to use RSA 
(asymetric encryption) to transmit a randomly-generated key for the current session, to your application and use it for the AES encryption.

# Usage

**Encrypt in C#, decrypt in PHP:**

```c#
  AES Aes = new AES();
  string EncryptedString = Aes.EncryptAESToString("Your input text here!", "*t+=L[8JM00NAlal");
  // System.Web.HttpUtility.UrlEncode requires reference to System.Web.dll
  return System.Web.HttpUtility.UrlEncode(EncryptedString)
```

```php
  // rawurldecode() must be used to ensure that no + will be converted into spaces from the base64 string.
  $Data = rawurldecode($_POST['data']);
  $Decrypted = Crypto::DecryptAES($Data, "*t+=L[8JM00NAlal");
  echo $Decrypted;
```

**Encrypt in PHP, decrypt in C#:**

```php
  $Data = Crypto::EncryptAES($Data, "*t+=L[8JM00NAlal");
  echo $Data;
```

```c#
  AES Aes = new AES();
  string EncryptedString = "the Base64 string you receive from the PHP script";
  return Aes.DecryptAESToString(EncryptedString, "*t+=L[8JM00NAlal");
```

# Things to pay attention to

* The encryption key must be 16 characters long if it's a string
* The encryption key must be 32 bytes long if it's a byte array (256 bits)
* The Base64 string must be URL encoded to be passed to a PHP script for decryption because there are special characters in it.
* The Base64 string must be decoded with rawurldecode() in order to prevent "+"s turning into spaces.
