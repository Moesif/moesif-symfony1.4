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

**The below methods to update user and company are accessible via the Moesif PHP API lib which Moesif Play Filter already imports as a dependency.**

## Update a Single User

Create or update a user profile in Moesif. 
The metadata field can be any customer demographic or other info you want to store.
Only the `user_id` field is required.
For details, visit the [PHP API Reference](https://www.moesif.com/docs/api?php#update-a-user).

```php
use MoesifApi\MoesifApiClient;
$client = new MoesifApiClient("Your Moesif Application Id");
$api = $client->getApi();

// Campaign object is optional, but useful if you want to track ROI of acquisition channels
// See https://www.moesif.com/docs/api#update-a-user for campaign schema
$campaign = new Models\CampaignModel();
$campaign->utmSource = "google";
$campaign->utmCampaign = "cpc";
$campaign->utmMedium = "adwords";
$campaign->utmContent = "api+tooling";
$campaign->utmTerm = "landing";

$user = new Models\UserModel();
$user->userId = "12345";
$user->companyId = "67890"; // If set, associate user with a company object
$user->campaign = $campaign;

// metadata can be any custom object
$user->metadata = array(
        "email" => "john@acmeinc.com",
        "first_name" => "John",
        "last_name" => "Doe",
        "title" => "Software Engineer",
        "sales_info" => array(
            "stage" => "Customer",
            "lifetime_value" => 24000,
            "account_owner" => "mary@contoso.com"
        )
    );

$api->updateUser($user);
```

## Update Users in Batch

Similar to updateUser, but used to update a list of users in one batch. 
Only the `user_id` field is required.
For details, visit the [PHP API Reference](https://www.moesif.com/docs/api?php#update-users-in-batch).

```php
use MoesifApi\MoesifApiClient;
$client = new MoesifApiClient("Your Moesif Application Id");
$api = $client->getApi();

$userA = new Models\UserModel();
$userA->userId = "12345";
$userA->companyId = "67890"; // If set, associate user with a company object
$userA->campaign = $campaign;

// metadata can be any custom object
$userA->metadata = array(
        "email" => "john@acmeinc.com",
        "first_name" => "John",
        "last_name" => "Doe",
        "title" => "Software Engineer",
        "sales_info" => array(
            "stage" => "Customer",
            "lifetime_value" => 24000,
            "account_owner" => "mary@contoso.com"
        )
    );

$users = array($userA)
$api->updateUsersBatch($user);
```

## Update a Single Company

Create or update a company profile in Moesif.
The metadata field can be any company demographic or other info you want to store.
Only the `company_id` field is required.
For details, visit the [PHP API Reference](https://www.moesif.com/docs/api?php#update-a-company).

```php
use MoesifApi\MoesifApiClient;
$client = new MoesifApiClient("Your Moesif Application Id");
$api = $client->getApi();

// Campaign object is optional, but useful if you want to track ROI of acquisition channels
// See https://www.moesif.com/docs/api#update-a-company for campaign schema
$campaign = new Models\CampaignModel();
$campaign->utmSource = "google";
$campaign->utmCampaign = "cpc";
$campaign->utmMedium = "adwords";
$campaign->utmContent = "api+tooling";
$campaign->utmTerm = "landing";

$company = new Models\CompanyModel();
$company->companyId = "67890";
$company->companyDomain = "acmeinc.com";
$company->campaign = $campaign;

// metadata can be any custom object
$company->metadata = array(
        "org_name" => "Acme, Inc",
        "plan_name" => "Free",
        "deal_stage" => "Lead",
        "mrr" => 24000,
        "demographics" => array(
            "alexa_ranking" => 500000,
            "employee_count" => 47
        )
    );

$api->updateCompany($company);
```

## Update Companies in Batch

Similar to updateCompany, but used to update a list of companies in one batch. 
Only the `company_id` field is required.
For details, visit the [PHP API Reference](https://www.moesif.com/docs/api?php#update-companies-in-batch).

```php
use MoesifApi\MoesifApiClient;
$client = new MoesifApiClient("Your Moesif Application Id");
$api = $client->getApi();

$companyA = new Models\CompanyModel();
$companyA->companyId = "67890";
$companyA->companyDomain = "acmeinc.com";
$companyA->campaign = $campaign;

// metadata can be any custom object
$companyA->metadata = array(
        "org_name" => "Acme, Inc",
        "plan_name" => "Free",
        "deal_stage" => "Lead",
        "mrr" => 24000,
        "demographics" => array(
            "alexa_ranking" => 500000,
            "employee_count" => 47
        )
    );

$companies = array($companyA)
$api->updateCompaniesBatch($companies);
```

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
