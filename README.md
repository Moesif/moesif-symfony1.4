## Moesif Symfony(1.4) SDK

Official SDK for PHP Symfony(1.4) to automatically capture incoming HTTP traffic.

[![Built For][ico-built-for]][link-built-for]
[![Latest Version][ico-version]][link-package]
[![Total Downloads][ico-downloads]][link-downloads]
[![Software License][ico-license]][link-license]
[![Source Code][ico-source]][link-source]

[Source Code on GitHub](https://github.com/Moesif/moesif-symfony1.4)

## How to install

Via Composer

```bash
$ composer require moesif/moesif-symfony1.4
```
or add 'moesif/moesif-symfony1.4' to your composer.json file accordingly.

## How to enable MoesifFilter

Create a custom filter `MyCustomFilter` which extends MoesifFilter and you would be able to view all the API calls being captured.

```php
<?php

use MoesifFilter;

class MyCustomFilter extends MoesifFilter {

}
```


Update the filters.yml files in your application to enable capturing API calls.

`config/filters.yml`

```yaml
MyCustomFilter:  
  class: MyCustomFilter
  param:
    applicationId: Your Moesif Application Id
    debug: 'true'
    logBody: 'true'
```

`apps/frontend/config/filters.yml`

```yaml
MyCustomFilter:  
  class: MyCustomFilter
  debug: 'true'
  param:
    applicationId: Your Moesif Application Id
    debug: 'true'
    logBody: 'true'
```

Your Moesif Application Id can be found in the [_Moesif Portal_](https://www.moesif.com/).
After signing up for a Moesif account, your Moesif Application Id will be displayed during the onboarding steps. 

You can always find your Moesif Application Id at any time by logging 
into the [_Moesif Portal_](https://www.moesif.com/), click on the top right menu,
and then clicking _Installation_.

## Configuration Options

#### applicationId
Type: `String`
Required, a string that identifies your application.

#### debug
Type: `Boolean`
Optional, If true, will print debug messages using [sfFileLogger](http://www.symfony-project.org/api/1_4/sfFileLogger.html)

#### identifyUserId
Type: `String`
Optional, (Highly Recommend) Moesif automatically try to obtain end userId, In case you use a non standard way of injecting user, you can do so with by setting `X-Moesif-User-Id` header in the API request.

#### identifyCompanyId
Type: `String`
Optional, Set `X-Moesif-Company-Id` header in the API request to set the companyId for the event.

#### identifySessionToken
Type: `String`
Optional, Moesif automatically sessionizes by processing at your data, but you can override this by setting `X-Moesif-Session-Token` header in the API request if you're not happy with the results.

#### maskRequestHeaders
Type: `String`
Optional, Set `X-Moesif-Mask-Request-Headers` headers, which is a comma seperated string, which will mask your sensitive request headers.

For example - `X-Moesif-Mask-Request-Headers` should be set to `header1, header2` to mask `header1` and `header2` from your API call request header.

#### maskRequestBody
Type: `String`
Optional, Set `X-Moesif-Mask-Request-Body` headers, which is a comma seperated string, which will mask your sensitive request body.

For example - `X-Moesif-Mask-Request-Body` should be set to `fieldA, fieldB` to mask `fieldA` and `fieldB` from your API call request body.

#### maskResponseHeaders
Type: `String`
Optional, Set `X-Moesif-Mask-Response-Headers` headers, which is a comma seperated string, which will mask your sensitive response headers.

For example - `X-Moesif-Mask-Response-Headers` should be set to `header1, header2` to mask `header1` and `header2` from your API call response header.

#### maskResponseBody
Type: `String`
Optional, Set `X-Moesif-Mask-Response-Body` headers, which is a comma seperated string, which will mask your sensitive response body.

For example - `X-Moesif-Mask-Response-Body` should be set to `fieldA, fieldB` to mask `fieldA` and `fieldB` from your API call response body.

#### skip
Type: `String`
Optional, Set `X-Moesif-Skip` header in the API request to skip sending a particular event to Moesif if the header value matches the URI for the request.

For example - if the request URI is - `http://localhost:8888/index.php` and you've set the `X-Moesif-Skip` header to `index`, it will skip sending that particular event to Moesif.

#### logBody
Type: `Boolean`
Optional, If set to false will not log request and response body to Moesif.

## An Example Symfony 1.4 App with Moesif Integrated

[Moesif Symfony-1.4 Example](https://github.com/Moesif/moesif-symfony1.4-example)

## Other integrations

To view more documentation on integration options, please visit __[the Integration Options Documentation](https://www.moesif.com/docs/getting-started/integration-options/).__

[ico-built-for]: https://img.shields.io/badge/built%20for-symfony1.4-blue.svg
[ico-version]: https://img.shields.io/packagist/v/moesif/moesif-symfony1.4.svg
[ico-downloads]: https://img.shields.io/packagist/dt/moesif/moesif-symfony1.4.svg
[ico-license]: https://img.shields.io/badge/License-Apache%202.0-green.svg
[ico-source]: https://img.shields.io/github/last-commit/moesif/moesif-symfony1.4.svg?style=social

[link-built-for]: https://symfony.com/legacy
[link-package]: https://packagist.org/packages/moesif/moesif-symfony1.4
[link-downloads]: https://packagist.org/packages/moesif/moesif-symfony1.4
[link-license]: https://raw.githubusercontent.com/Moesif/moesif-symfony1.4/master/LICENSE
[link-source]: https://github.com/Moesif/moesif-symfony1.4
