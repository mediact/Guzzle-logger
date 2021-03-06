[![codecov.io](https://codecov.io/github/gmponos/Guzzle-logger/coverage.svg?branch=master)](https://codecov.io/github/gmponos/Guzzle-logger?branch=master)
[![Build Status](https://travis-ci.org/gmponos/Guzzle-logger.svg?branch=master)](https://travis-ci.org/gmponos/Guzzle-logger)

# GuzzleHttpLogger Middleware

This is a middleware for the guzzle that automatically logs every request and response using a PSR-3 logger.

## Install

Via Composer

``` bash
$ composer require gmponos/guzzle-http-logger
```

## Usage

### Simple usage

``` php
use Gmponos\GuzzleLogger\Middleware\LoggerMiddleware;
use GuzzleHttp\HandlerStack;

$logger = new Logger();  //A new PSR-3 Logger like Monolog
$stack = new HandlerStack();
$stack->push(new LoggerMiddleware($logger));
$client = new GuzzleHttp\Client([
    'handler' => $stack,
]);
```

From now on each request you make and response that you receive using the ``$client`` will be logged.
The default levels that the middleware uses for logging are the following.

- Requests are logged with level debug.
- Request statistics are logged with level debug.
- Responses with http status code < 400 with level debug.
- Responses with http status code < 500 with level error.
- Responses with http status code >= 500 with level critical.

### Advanced initialization

The signature of the LoggerMiddleware class is the following:

``LoggerMiddleware(LoggerInterface $logger, $logRequests = true, $logStatistics = false, array $thresholds = [])``

- **logger** - The PSR-3 logger to use for logging.
- **logRequests** - By default the middleware is set to log every request and response. If you wish that to log only the requests and responses that you retrieve a status code above 4xx set this as false.
- **logRequests** - If you set logRequests as true and this as true then guzzle will also log statistics about the requests.
- **thresholds** - An array that you may use to change the thresholds of logging the responses. 

### Using options on each request

You can set on each request options about your log.

```php
$client->get("/", [
    'log' => [
        'requests' => true,
        'statistics' => true,
        'error_threshold' => null,
        'warning_threshold' => null,
        'levels' => [
            400 => 'info'
            401 => 'warning'
            ...
        ]
    ]
]);
```

- ``requests`` Do not log anything unless if the request is above the threshold or inside the levels.
- ``statistics`` if the requests variable is true and this is also true the logger will log statistics about the request
- ``levels`` set custom log levels for each response status code

## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CONDUCT](CONDUCT.md) for details.

## Credits

- [George Mponos](gmponos@gmail.com)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## Todo
 - Decouple the format of the record from the middleware.
 - More tests
