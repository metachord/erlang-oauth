# An Erlang OAuth implementation

## Quick start (client usage)

    $ make
    ...
    $ erl -pa ebin -s crypto -s inets -s ssl
    ...
    1> rr(oauth).
    ...
    2> Consumer = #oauth_consumer{key = "key", secret = "secret", method = hmac_sha1}.
    ...
    3> RequestTokenURL = "http://term.ie/oauth/example/request_token.php".
    ...
    4> {ok, RequestTokenResponse} = oauth:get(RequestTokenURL, [], Consumer).
    ...
    5> RequestTokenParams = oauth:params_decode(RequestTokenResponse).
    ...
    6> RequestToken = oauth:token(RequestTokenParams).
    ...
    7> RequestTokenSecret = oauth:token_secret(RequestTokenParams).
    ...
    8> AccessTokenURL = "http://term.ie/oauth/example/access_token.php".
    ...
    9> {ok, AccessTokenResponse} = oauth:get(AccessTokenURL, [], Consumer, RequestToken, RequestTokenSecret).
    ...
    10> AccessTokenParams = oauth:params_decode(AccessTokenResponse).
    ...
    11> AccessToken = oauth:token(AccessTokenParams).
    ...
    12> AccessTokenSecret = oauth:token_secret(AccessTokenParams).
    ...
    13> URL = "http://term.ie/oauth/example/echo_api.php".
    ...
    14> {ok, Response} = oauth:get(URL, [{"hello", "world"}], Consumer, AccessToken, AccessTokenSecret).
    ...
    15> oauth:params_decode(Response).
    ...


## Notes

Consumer credentials are represented as follows:

    #oauth_consumer{key = Key::string(), secret = Secret::string(), method = plaintext}

    #oauth_consumer{key = Key::string(), secret = Secret::string(), method = hmac_sha1}

    #oauth_consumer{key = Key::string(), secret = RSAPrivateKeyPath::string(), method = rsa_sha1}  % client side

    #oauth_consumer{key = Key::string(), secret = RSACertificatePath::string(), method = rsa_sha1}  % server side


Erlang/OTP R14B or greater is required for RSA-SHA1

The percent encoding/decoding implementations are based on [ibrowse](https://github.com/cmullaparthi/ibrowse)

Example client/server code: [github.com/tim/erlang-oauth-examples](https://github.com/tim/erlang-oauth-examples)

Unit tests: [github.com/tim/erlang-oauth-tests](https://github.com/tim/erlang-oauth-tests)
