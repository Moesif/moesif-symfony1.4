## Moesif Symfony 1.4 SDK

Official SDK for PHP Symfony 1.x to automatically capture API traffic and send to the Moesif API Analytics platform.

[![Built For][ico-built-for]][link-built-for]
[![Latest Version][ico-version]][link-package]
[![Software License][ico-license]][link-license]
[![Source Code][ico-source]][link-source]

[Source Code on GitHub](https://github.com/Moesif/moesif-symfony1.4)

## How to install

Via Composer

```bash
$ composer require moesif/moesif-symfony1.4
```
or add `moesif/moesif-symfony1.4` to your composer.json file accordingly.

## How to enable MoesifFilter

Create a custom filter `MyCustomFilter` which extends MoesifFilter and you would be able to view all the API calls being captured.

```php
<?php

use MoesifFilter;

class MyCustomFilter extends MoesifFilter {

    /**
     * Example implementation returning the customer's user_id.
     */
    public function identifyUserId($request, $response){
        $user = $this->getContext()->getUser();
        return $user->getAttribute("user_id");
    }

    /**
     * Example implementation returning the customer's session token/auth token.
     */
    function identifySessionToken($request, $response){
        return $request->getHttpHeader('Authorization');
    }

}
```


Add your filter to the `config/filters.yml` file in your application with
your Moesif Application Id.

```yaml
MyCustomFilter:  
  class: MyCustomFilter
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

## YAML Configuration Options

#### __`applicationId`__
Type: `String`
Required, a string that identifies your application.

#### __`debug`__
Type: `Boolean`
Optional, If true, will print debug messages using [sfFileLogger](http://www.symfony-project.org/api/1_4/sfFileLogger.html)

#### __`logBody`__
Type: `Boolean`
Optional, Default true, Set to false to remove logging request and response body to Moesif.

## Filter Class Configuration Options

#### __`identifyUserId`__
Type: `($request, $response) => String`
Optional, a function that takes a $request and $response and return a string for userId. This enables Moesif to attribute API requests to individual unique users so you can understand who calling your API. This can be used simultaneously with `identifyCompanyId` to track both individual customers and also the companies they are a part of.

#### __`identifyCompanyId`__
Type: `($request, $response) => String`
Optional, a function that takes a $request and $response and return a string for companyId. If your business is B2B, this enables Moesif to attribute API requests to specific companies or organizations so you can understand which accounts are calling your API. This can be used simultaneously with `identifyUserId` to track both individual customers and the companies their a part of.

#### __`identifySessionToken`__
Type: `($request, $response) => String`
Optional, a function that takes a $request and $response and return a string for session token/auth token. Moesif automatically sessionizes by processing your API data, but you can override this via identifySessionId if you're not happy with the results.

#### __`maskRequestHeaders`__
Type: `$headers => $headers`
Optional, a function that takes a $headers, which is an associative array, and
returns an associative array with your sensitive headers removed/masked.

#### __`maskRequestBody`__
Type: `$body => $body`
Optional, a function that takes a $body, which is an associative array representation of JSON, and
returns an associative array with any information removed.

#### __`maskResponseHeaders`__
Type: `$headers => $headers`
Optional, same as above, but for Responses.

#### __`maskResponseBody`__
Type: `$body => $body`
Optional, same as above, but for Responses.

#### __`getMetadata`__
Type: `($request, $response) => Associative Array`
Optional, a function that takes a $request and $response and returns $metdata which is an associative array representation of JSON.

#### __`skip`__
Type: `($request, $response) => String`
Optional, a function that takes a $request and $response and returns true if this API call should be not be sent to Moesif.

## An Example Symfony 1.4 App with Moesif Integrated

[Moesif Symfony-1.4 Example](https://github.com/Moesif/moesif-symfony1.4-example)

## Other integrations

To view more documentation on integration options, please visit __[the Integration Options Documentation](https://www.moesif.com/docs/getting-started/integration-options/).__

[ico-built-for]: https://img.shields.io/badge/built%20for-symfony1.4-blue.svg
[ico-version]: https://img.shields.io/packagist/v/moesif/moesif-symfony1.4.svg
[ico-license]: https://img.shields.io/badge/License-Apache%202.0-green.svg
[ico-source]: https://img.shields.io/github/last-commit/moesif/moesif-symfony1.4.svg?style=social

[link-built-for]: https://symfony.com/legacy
[link-package]: https://packagist.org/packages/moesif/moesif-symfony1.4
[link-license]: https://raw.githubusercontent.com/Moesif/moesif-symfony1.4/master/LICENSE
[link-source]: https://github.com/Moesif/moesif-symfony1.4
