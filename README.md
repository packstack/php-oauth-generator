# php-oauth-generator

**Author:** Thomas Hunter II

**Source:** https://thomashunter.name/blog/generate-oauth-consumer-key-and-shared-secrets-using-php/

```php
function gen_oauth_creds() {
	// Get a whole bunch of random characters from the OS
	$fp = fopen('/dev/urandom','rb');
	$entropy = fread($fp, 32);
	fclose($fp);

	// Takes our binary entropy, and concatenates a string which represents the current time to the microsecond
	$entropy .= uniqid(mt_rand(), true);

	// Hash the binary entropy
	$hash = hash('sha512', $entropy);

	// Base62 Encode the hash, resulting in an 86 or 85 character string
	$hash = gmp_strval(gmp_init($hash, 16), 62);

	// Chop and send the first 80 characters back to the client
	return array(
		'consumer_key' => substr($hash, 0, 32),
		'shared_secret' => substr($hash, 32, 48)
	);
}
```

Example output: 

```
lInXLgx6HbF9FFq1ZQN8iSEnhzO3JVuf : 6kvwVhDGUQGXFawmulAhvORRV8HpZy5OMqMVH7xqwkLcvTbo
uFKE99LWQ8PDdBFQiO6fUhRRLoagFcDT : RGa8MqS4NZtAbWM8Voloi9kSVPg5AoKc5kySZxoYtpkOFgHU
NZqgHW5oOkxHxc5OgVXkJGQ0Kus3rOjz : QQsfDh5Q7S1BB8UHiB0Ni3okKn8lEEbeDx1k4k2OjT2jWuzr
```
