# GoogleIDToken

GoogleIDToken currently provides a single useful class "Validator", which provides a single method "#check", which parses and validates an ID Token allegedly generated by Google auth servers.

Creating a new validator takes a single optional hash argument. If the hash has an entry for :x509_key, that value is taken to be a key as created by OpenSSL::X509::Certificate.new, and the token is validated using that key.  If there is no such entry, the keys are fetched from the Google certs endpoint https://www.googleapis.com/oauth2/v1/certs. The certificates are cached for some time (default: 1 day) which can be configured by adding the :expiry argument to the initialization hash (in seconds).

## Installation

    gem install google-id-token

## Examples

``` ruby
validator = GoogleIDToken::Validator.new(expiry: 1800)
begin
  payload = validator.check(token, required_audience, optional_client_id)
  email = payload['email']
rescue GoogleIDToken::ValidationError => e
  report "Cannot validate: #{e}"
end


cert = OpenSSL::X509::Certificate.new(File.read('my-cert.pem'))
validator = GoogleIDToken::Validator.new(x509_cert: cert)
begin
  payload = validator.check(token, required_audience, optional_client_id)
  email = payload['email']
rescue GoogleIDToken::ValidationError => e
  report "Cannot validate: #{e}"
end
```

