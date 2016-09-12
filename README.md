# PostmarkBundle
Symfony3 bundle for [Postmark](http://postmarkapp.com) API [![Build Status](https://secure.travis-ci.org/miguel250/PostmarkBundle.png?branch=master)](http://travis-ci.org/miguel250/PostmarkBundle)

**This is a ridiculously minor update of Miguel Perez's no-longer maintained code from https://github.com/miguel250/PostmarkBundle to allow for Symfony 3 compatibility. All credit/kudos/thanks for the work should obviously go to him**

## Setup

**Using Composer**
Add PostmarkBundle in your composer.json:

``` bash
$ php composer.phar require stephenfarrell/postmark-bundle
```

``` bash
$ php composer.phar update stephenfarrell/postmark-bundle
```

**Using Submodule**

    git submodule add https://github.com/stephenfarrell/PostmarkBundle.git vendor/bundles/MZ/PostmarkBundle
    git submodule add https://github.com/kriswallsmith/Buzz.git  vendor/buzz

**Add the MZ namespace to autoloader**
You can skip this when using Composer

``` php
<?php
   // app/autoload.php
   $loader->registerNamespaces(array(
    // ...
    'MZ'               => __DIR__.'/../vendor/bundles',
    'Buzz'             => __DIR__.'/../vendor/buzz/lib',
  ));
```
**Add PostmarkBundle to your application kernel**

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new MZ\PostmarkBundle\MZPostmarkBundle(),
    );
}
```

**Enable Postmark in config.yml**
``` yml
mz_postmark:
    api_key: API KEY
    from_email: info@my-app.com
    from_name: My App, Inc
    use_ssl: true
    timeout: 5
```

## Usage

**Message Service**
``` php
<?php
$message  = $this->get('postmark.message');
$message->addTo('test@gmail.com', 'Test Test');
$message->setSubject('subject');
$message->setHTMLMessage('<b>email body</b>');
$message->addAttachment(new Symfony\Component\HttpFoundation\File\File(__FILE__));
$response = $message->send();

$message->addTo('test2@gmail.com', 'Test2 Test');
$message->setSubject('subject2');
$message->setHTMLMessage('<b>email body</b>');
$message->addAttachment(new Symfony\Component\HttpFoundation\File\File(__FILE__), 'usethisfilename.php', 'text/plain');
$response = $message->send();
?>
```

**Sending in batch**
``` php
<?php
$message  = $this->get('postmark.message');
$message->addTo('test@gmail.com', 'Test Test');
$message->setSubject('subject');
$message->setHTMLMessage('<b>email body</b>');
$message->queue(); // Queue the message instead of sending it directly

$message->addTo('test2@gmail.com', 'Test2 Test');
$message->setSubject('subject2');
$message->setHTMLMessage('<b>email body</b>');
$responses = $message->send(); // Send both messages, note that you'll get an array of json responses instead of just the json response
?>
```