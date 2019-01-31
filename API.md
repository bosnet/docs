FORMAT: 1A
HOST: https://mainnet.blockchainos.org

# Sebak Client API
SEBAK, the next BOScoin network with ISAAC consensus protocol. 

## SEBAK API supports [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
- Request only accepted methods
    - [**GET**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)
    - [**HEAD**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD)
    - [**POST**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)
- Except automatic set up by user-agent ,request Header accepts only;
    - [**Accept**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)
    - [**Accept-Language**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language)
    - [**Content-Language**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)
    - [**Content-type**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type): you should check detail information below about content-type.
    - [**DPR**](https://httpwg.org/http-extensions/client-hints.html#dpr)
    - [**Save-data**](https://httpwg.org/http-extensions/client-hints.html#save-data)
    - [**Viewport-width**](https://httpwg.org/http-extensions/client-hints.html#viewport-width)
    - [**Width**](https://httpwg.org/http-extensions/client-hints.html#width)
- The only allowed values for the Content-Type header are:
    - ```application/x-www-form-urlencoded```
    - ```multipart/form-data```
    - ```text/plain```

<!-- partial(API_v1/accounts.md) -->
<!-- partial(API_v1/models.md) -->
<!-- partial(API_v1/transactions.md) -->

<!-- include(API_v1/paging.md) -->
<!-- include(API_v1/models.md) -->
<!-- include(API_v1/accounts.md) -->
<!-- include(API_v1/transactions.md) -->
<!-- include(API_v1/blocks.md) -->
