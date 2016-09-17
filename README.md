Json Utils
==========

* JSONMinify: properly Minifies JSON to be used on Github HMAC verification, specially for validation of HMAC signatures of Github Enterprise Webhook hooks.

JSONMinify.minify(payload)
-----

This is needed for verifying Github Enterprise HMAC Webhook signatures. Since the payload is a JSON payload, it needs to be the minified version of the JSON to properly generate a valid HMAC SHA1 value of the payload. Results are the same as using http://beautifytools.com/json-minifier.php.

```java
  /**
   * Computes a RFC 2104-compliant HMAC digest using SHA1 of a payloadFrom with a given key
   * (secret).
   *
   * @param payload is the payload provided by the user.
   * @param secret is the secret used by the user.
   * @return HMAC digest of payloadFrom using secret as key. Will return COMPUTED_INVALID_SIGNATURE
   *         on any exception during computation.
   * @throws InvalidKeyException when the key is invalid.
   */
  public static String sha1(String payload, String secret) throws InvalidKeyException {
    payload = payload.startsWith("[") || payload.startsWith("{") ? JSONMinify.minify(payload) : payload;

    final SecretKeySpec keySpec = new SecretKeySpec(secret.getBytes(UTF_8), HMAC_SHA1_ALGORITHM);
    try {
      Mac mac = Mac.getInstance(HMAC_SHA1_ALGORITHM);
      mac.init(keySpec);
      final byte[] rawHMACBytes = mac.doFinal(payload.getBytes(UTF_8));

      return Hex.encodeHexString(rawHMACBytes);

    } catch (NoSuchAlgorithmException e) {
      return null;  // not happening.
    }
  }
```

Gradle Dependency Coodinate
-------

```groovy
  compile 'com.github.marcellodesales:json-utils:1.0.0'
```

Maven Dependency Coordinate
-----

* Maven

```xml
<dependency>
  <groupId>com.github.marcellodesales</groupId>
  <artifactId>json-utils</artifactId>
  <version>1.0.0</version>
</dependency>
```

Dev
=====

* Initial Setup in Maven Central: https://issues.sonatype.org/browse/OSSRH-25026
