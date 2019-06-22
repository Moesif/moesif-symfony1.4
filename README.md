## Moesif Symfony(1.4) SDK

Official SDK for PHP Symfony(1.4) to automatically capture incoming HTTP traffic.

## How to install

Via Composer

```bash
$ composer require moesif/moesif-laravel
```
or add 'moesif/moesif-laravel' to your composer.json file accordingly.

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

## Other integrations

To view more documentation on integration options, please visit __[the Integration Options Documentation](https://www.moesif.com/docs/getting-started/integration-options/).__