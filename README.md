![Build Status](https://github.com/firebase/php-jwt/actions/workflows/tests.yml/badge.svg)
[![Latest Stable Version](https://poser.pugx.org/firebase/php-jwt/v/stable)](https://packagist.org/packages/firebase/php-jwt)
[![Total Downloads](https://poser.pugx.org/firebase/php-jwt/downloads)](https://packagist.org/packages/firebase/php-jwt)
[![License](https://poser.pugx.org/firebase/php-jwt/license)](https://packagist.org/packages/firebase/php-jwt)

PHP-JWT
=======
Uma biblioteca simples para codificar e decodificar JSON Web Tokens (JWT) em PHP, em conformidade com [RFC 7519](https://tools.ietf.org/html/rfc7519).

Instalação
------------

Use o composer para gerenciar suas dependências e baixe o PHP-JWT:

```bash
composer require firebase/php-jwt
```

Opcionalmente, instale o pacote `paragonie/sodium_compat` do composer se o seu
php é < 7.2 ou não tem libsodium instalado:

```bash
composer require paragonie/sodium_compat
```

Examplo
-------
```php
use Firebase\JWT\JWT;
use Firebase\JWT\Key;

$key = 'example_key';
$payload = [
    'iss' => 'http://example.org',
    'aud' => 'http://example.com',
    'iat' => 1356999524,
    'nbf' => 1357000000
];

/**
 * IMPORTANTE:
 * Você deve especificar algoritmos suportados para seu aplicativo. Ver
 * https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40
 * para obter uma lista de algoritmos compatíveis com especificações.
 */
$jwt = JWT::encode($payload, $key, 'HS256');
$decoded = JWT::decode($jwt, new Key($key, 'HS256'));

print_r($decoded);

/*
NOTA: Isso agora será um objeto em vez de uma matriz associativa. Obter
uma matriz associativa, você precisará convertê-la como tal:
*/

$decoded_array = (array) $decoded;

/**
  * Você pode adicionar uma margem de manobra para contabilizar quando há uma diferença de horário entre
  * os servidores de assinatura e verificação. Recomenda-se que esta margem de manobra seja
  * não ser maior do que alguns minutos.
  *
  * Fonte: http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html#nbfDef
  */
  
JWT::$leeway = 60; // $leeway in seconds
$decoded = JWT::decode($jwt, new Key($key, 'HS256'));
```
Example with RS256 (openssl)
----------------------------
```php
use Firebase\JWT\JWT;
use Firebase\JWT\Key;

$privateKey = <<<EOD
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAuzWHNM5f+amCjQztc5QTfJfzCC5J4nuW+L/aOxZ4f8J3Frew
M2c/dufrnmedsApb0By7WhaHlcqCh/ScAPyJhzkPYLae7bTVro3hok0zDITR8F6S
JGL42JAEUk+ILkPI+DONM0+3vzk6Kvfe548tu4czCuqU8BGVOlnp6IqBHhAswNMM
78pos/2z0CjPM4tbeXqSTTbNkXRboxjU29vSopcT51koWOgiTf3C7nJUoMWZHZI5
HqnIhPAG9yv8HAgNk6CMk2CadVHDo4IxjxTzTTqo1SCSH2pooJl9O8at6kkRYsrZ
WwsKlOFE2LUce7ObnXsYihStBUDoeBQlGG/BwQIDAQABAoIBAFtGaOqNKGwggn9k
6yzr6GhZ6Wt2rh1Xpq8XUz514UBhPxD7dFRLpbzCrLVpzY80LbmVGJ9+1pJozyWc
VKeCeUdNwbqkr240Oe7GTFmGjDoxU+5/HX/SJYPpC8JZ9oqgEA87iz+WQX9hVoP2
oF6EB4ckDvXmk8FMwVZW2l2/kd5mrEVbDaXKxhvUDf52iVD+sGIlTif7mBgR99/b
c3qiCnxCMmfYUnT2eh7Vv2LhCR/G9S6C3R4lA71rEyiU3KgsGfg0d82/XWXbegJW
h3QbWNtQLxTuIvLq5aAryV3PfaHlPgdgK0ft6ocU2de2FagFka3nfVEyC7IUsNTK
bq6nhAECgYEA7d/0DPOIaItl/8BWKyCuAHMss47j0wlGbBSHdJIiS55akMvnAG0M
39y22Qqfzh1at9kBFeYeFIIU82ZLF3xOcE3z6pJZ4Dyvx4BYdXH77odo9uVK9s1l
3T3BlMcqd1hvZLMS7dviyH79jZo4CXSHiKzc7pQ2YfK5eKxKqONeXuECgYEAyXlG
vonaus/YTb1IBei9HwaccnQ/1HRn6MvfDjb7JJDIBhNClGPt6xRlzBbSZ73c2QEC
6Fu9h36K/HZ2qcLd2bXiNyhIV7b6tVKk+0Psoj0dL9EbhsD1OsmE1nTPyAc9XZbb
OPYxy+dpBCUA8/1U9+uiFoCa7mIbWcSQ+39gHuECgYAz82pQfct30aH4JiBrkNqP
nJfRq05UY70uk5k1u0ikLTRoVS/hJu/d4E1Kv4hBMqYCavFSwAwnvHUo51lVCr/y
xQOVYlsgnwBg2MX4+GjmIkqpSVCC8D7j/73MaWb746OIYZervQ8dbKahi2HbpsiG
8AHcVSA/agxZr38qvWV54QKBgCD5TlDE8x18AuTGQ9FjxAAd7uD0kbXNz2vUYg9L
hFL5tyL3aAAtUrUUw4xhd9IuysRhW/53dU+FsG2dXdJu6CxHjlyEpUJl2iZu/j15
YnMzGWHIEX8+eWRDsw/+Ujtko/B7TinGcWPz3cYl4EAOiCeDUyXnqnO1btCEUU44
DJ1BAoGBAJuPD27ErTSVtId90+M4zFPNibFP50KprVdc8CR37BE7r8vuGgNYXmnI
RLnGP9p3pVgFCktORuYS2J/6t84I3+A17nEoB4xvhTLeAinAW/uTQOUmNicOP4Ek
2MsLL2kHgL8bLTmvXV4FX+PXphrDKg1XxzOYn0otuoqdAQrkK4og
-----END RSA PRIVATE KEY-----
EOD;

$publicKey = <<<EOD
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuzWHNM5f+amCjQztc5QT
fJfzCC5J4nuW+L/aOxZ4f8J3FrewM2c/dufrnmedsApb0By7WhaHlcqCh/ScAPyJ
hzkPYLae7bTVro3hok0zDITR8F6SJGL42JAEUk+ILkPI+DONM0+3vzk6Kvfe548t
u4czCuqU8BGVOlnp6IqBHhAswNMM78pos/2z0CjPM4tbeXqSTTbNkXRboxjU29vS
opcT51koWOgiTf3C7nJUoMWZHZI5HqnIhPAG9yv8HAgNk6CMk2CadVHDo4IxjxTz
TTqo1SCSH2pooJl9O8at6kkRYsrZWwsKlOFE2LUce7ObnXsYihStBUDoeBQlGG/B
wQIDAQAB
-----END PUBLIC KEY-----
EOD;

$payload = [
    'iss' => 'example.org',
    'aud' => 'example.com',
    'iat' => 1356999524,
    'nbf' => 1357000000
];

$jwt = JWT::encode($payload, $privateKey, 'RS256');
echo "Encode:\n" . print_r($jwt, true) . "\n";

$decoded = JWT::decode($jwt, new Key($publicKey, 'RS256'));

/*
 NOTA: Isso agora será um objeto em vez de uma matriz associativa. Obter
 uma matriz associativa, você precisará convertê-la como tal:
*/

$decoded_array = (array) $decoded;
echo "Decode:\n" . print_r($decoded_array, true) . "\n";
```

Exemplo com uma frase secreta
-------------------------

```php
use Firebase\JWT\JWT;
use Firebase\JWT\Key;

// Sua senha
$passphrase = '[YOUR_PASSPHRASE]';

// Seu arquivo de chave privada com senha
// Pode ser gerado com "ssh-keygen -t rsa -m pem"
$privateKeyFile = '/path/to/key-with-passphrase.pem';

// Crie uma chave privada do tipo "recurso"
$privateKey = openssl_pkey_get_private(
    file_get_contents($privateKeyFile),
    $passphrase
);

$payload = [
    'iss' => 'example.org',
    'aud' => 'example.com',
    'iat' => 1356999524,
    'nbf' => 1357000000
];

$jwt = JWT::encode($payload, $privateKey, 'RS256');
echo "Encode:\n" . print_r($jwt, true) . "\n";

// Get public key from the private key, or pull from from a file.
$publicKey = openssl_pkey_get_details($privateKey)['key'];

$decoded = JWT::decode($jwt, new Key($publicKey, 'RS256'));
echo "Decode:\n" . print_r((array) $decoded, true) . "\n";
```

Exemplo com EdDSA (libsodium e assinatura Ed25519)
----------------------------
```php
use Firebase\JWT\JWT;
use Firebase\JWT\Key;

// Espera-se que as chaves públicas e privadas sejam codificadas em Base64. O último
// linha não vazia é usada para que as chaves possam ser geradas com
// sódio_crypto_sign_keypair(). As chaves secretas geradas por outras ferramentas podem
// precisa ser ajustado para corresponder à entrada esperada por libsodium.

$keyPair = sodium_crypto_sign_keypair();

$privateKey = base64_encode(sodium_crypto_sign_secretkey($keyPair));

$publicKey = base64_encode(sodium_crypto_sign_publickey($keyPair));

$payload = [
    'iss' => 'example.org',
    'aud' => 'example.com',
    'iat' => 1356999524,
    'nbf' => 1357000000
];

$jwt = JWT::encode($payload, $privateKey, 'EdDSA');
echo "Encode:\n" . print_r($jwt, true) . "\n";

$decoded = JWT::decode($jwt, new Key($publicKey, 'EdDSA'));
echo "Decode:\n" . print_r((array) $decoded, true) . "\n";
````

Usando JWKs
----------

```php
use Firebase\JWT\JWK;
use Firebase\JWT\JWT;

// Conjunto de chaves. A chave "chaves" é necessária. Por exemplo, a resposta JSON para
// este endpoint: https://www.gstatic.com/iap/verify/public_key-jwk
$jwks = ['keys' => []];

// JWK::parseKeySet($jwks) returns an associative array of **kid** to Firebase\JWT\Key
// objects. Pass this as the second parameter to JWT::decode.
JWT::decode($payload, JWK::parseKeySet($jwks));
```

Usando conjuntos de chaves em cache
---------------------

A classe `CachedKeySet` pode ser usada para buscar e armazenar em cache JWKS (JSON Web Key Sets) de um URI público.
Isso tem as seguintes vantagens:

1. The results are cached for performance.
2. If an unrecognized key is requested, the cache is refreshed, to accomodate for key rotation.
3. If rate limiting is enabled, the JWKS URI will not make more than 10 requests a second.

```php
use Firebase\JWT\CachedKeySet;
use Firebase\JWT\JWT;

// O URI para o JWKS do qual você deseja armazenar em cache os resultados
$jwksUri = 'https://www.gstatic.com/iap/verify/public_key-jwk';

// Crie um cliente HTTP (pode ser qualquer cliente HTTP compatível com PSR-7)
$httpClient = new GuzzleHttp\Client();

// Create an HTTP request factory (can be any PSR-17 compatible HTTP request factory)
$httpFactory = new GuzzleHttp\Psr\HttpFactory();

// Create a cache item pool (can be any PSR-6 compatible cache item pool)
$cacheItemPool = Phpfastcache\CacheManager::getInstance('files');

$keySet = new CachedKeySet(
    $jwksUri,
    $httpClient,
    $httpFactory,
    $cacheItemPool,
    null, // $expiresAfter int seconds to set the JWKS to expire
    true  // $rateLimit    true to enable rate limit of 10 RPS on lookup of invalid keys
);

$jwt = 'eyJhbGci...'; // Some JWT signed by a key from the $jwkUri above
$decoded = JWT::decode($jwt, $keySet);
```

Miscellaneous
-------------

#### Exception Handling

When a call to `JWT::decode` is invalid, it will throw one of the following exceptions:

```php
use Firebase\JWT\JWT;
use Firebase\JWT\SignatureInvalidException;
use Firebase\JWT\BeforeValidException;
use Firebase\JWT\ExpiredException;
use DomainException;
use InvalidArgumentException;
use UnexpectedValueException;

try {
    $decoded = JWT::decode($payload, $keys);
} catch (InvalidArgumentException $e) {
    // provided key/key-array is empty or malformed.
} catch (DomainException $e) {
    // provided algorithm is unsupported OR
    // provided key is invalid OR
    // unknown error thrown in openSSL or libsodium OR
    // libsodium is required but not available.
} catch (SignatureInvalidException $e) {
    // provided JWT signature verification failed.
} catch (BeforeValidException $e) {
    // provided JWT is trying to be used before "nbf" claim OR
    // provided JWT is trying to be used before "iat" claim.
} catch (ExpiredException $e) {
    // provided JWT is trying to be used after "exp" claim.
} catch (UnexpectedValueException $e) {
    // provided JWT is malformed OR
    // provided JWT is missing an algorithm / using an unsupported algorithm OR
    // provided JWT algorithm does not match provided key OR
    // provided key ID in key/key-array is empty or invalid.
}
```

Todas as exceções no namespace `Firebase\JWT` estendem `UnexpectedValueException` e podem ser simplificadas
assim:

```php
try {
    $decoded = JWT::decode($payload, $keys);
} catch (LogicException $e) {
    // errors having to do with environmental setup or malformed JWT Keys
} catch (UnexpectedValueException $e) {
    // errors having to do with JWT signature and claims
}
```

#### Transmitindo para matriz

O valor de retorno de `JWT::decode` é o objeto PHP genérico `stdClass`. Se você gostaria de lidar com arrays
em vez disso, você pode fazer o seguinte:

```php
// return type is stdClass
$decoded = JWT::decode($payload, $keys);

// cast to array
$decoded = json_decode(json_encode($decoded), true);
```

Tests
-----
Run the tests using phpunit:

```bash
$ pear install PHPUnit
$ phpunit --configuration phpunit.xml.dist
PHPUnit 3.7.10 by Sebastian Bergmann.
.....
Time: 0 seconds, Memory: 2.50Mb
OK (5 tests, 5 assertions)
```

Novas linhas em chaves privadas
-----

If your private key contains `\n` characters, be sure to wrap it in double quotes `""`
and not single quotes `''` in order to properly interpret the escaped characters.

License
-------
[3-Clause BSD](http://opensource.org/licenses/BSD-3-Clause).
