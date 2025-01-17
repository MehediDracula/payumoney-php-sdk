[![StyleCI](https://styleci.io/repos/51137750/shield)](https://styleci.io/repos/51137750)

# PayUMoney API for PHP

Simple library for accepting payments via [PayUMoney](https://www.payumoney.com/).

## Installation

To add this library to your project, run the command `composer require mehedi/payumoney-php-sdk:dev-master`.

## Usage

You'll find a minimal usage example below.

### Initialize purchase

```php
<?php
// purchase.php

use Mehedi\PayUMoney\PayUMoney;

require 'vendor/autoload.php';

$payumoney = new PayUMoney([
    'merchantId' => 'YOUR_MERCHANT_ID',
    'secretKey'  => 'YOUR_SECRET_KEY',
    'salt'       =>  'YOUR_SALT',
    'testMode'   => true
]);

// All of these parameters are required!
$params = [
    'txnid'       => 'A_UNIQUE_TRANSACTION_ID',
    'amount'      => 10.50,
    'productinfo' => 'A book',
    'firstname'   => 'Peter',
    'email'       => 'abc@example.com',
    'phone'       => '1234567890',
    'surl'        => 'http://localhost/payumoney-php/return.php',
    'furl'        => 'http://localhost/payumoney-php/return.php',
];

// Redirects to PayUMoney
$payumoney->initializePurchase($params)->send();
```

### Finalize purchase

```php
<?php
// return.php

use Razzbee\PayUMoney\PayUMoney;

require 'vendor/autoload.php';

$payumoney = new PayUMoney([
    'merchantId' => 'YOUR_MERCHANT_ID',
    'secretKey'  => 'YOUR_SECRET_KEY',
    'salt'       =>  'YOUR_SALT',
    'testMode'   => true
]);

$result = $payumoney->completePurchase($_POST);

if ($result->checksumIsValid() && $result->getStatus() === PayUMoney::STATUS_COMPLETED) {
  print 'Payment was successful.';
} else {
  print 'Payment was not successful.';
}
```

The `PurchaseResult` has a few more methods that might be useful:

```php
$result = $payumoney->completePurchase($_POST);

// Returns Complete, Pending, Failed or Tampered
$result->getStatus();

// Returns an array of all the parameters of the transaction
$result->getParams();

// Returns the ID of the transaction
$result->getTransactionId();

// Returns true if the checksum is correct
$result->checksumIsValid();
```
