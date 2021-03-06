Guzzle plugins
==============

> Clients created through the Guzzle service builder need to attach plugins in their `factory` method instead.

Guzzle plugins can be created and attached to your clients easily in your bundle.

For example, to attach the `CurlAuth` plugin to your client first create a service for the plugin:

    // MyBundle/Resources/config/services.xml

    <service id="example.client.curl_auth" class="%guzzle.plugin.curl_auth.class%">
        <argument>%example.client.curl_auth.username%</argument>
        <argument>%example.client.curl_auth.password%</argument>
    </service>

Next set the username and password parameters from your application's configuration:

    // MyBundle/DependencyInjection/MyBundleExtension.php

    $container->setParameter('example.client.curl_auth.username', $config['username']);
    $container->setParameter('example.client.curl_auth.password', $config['password']);

Then attach the plugin service to your client service through the `addSubscriber` method:

    // MyBundle/Resources/config/services.xml

    <service id="example.client" class="%guzzle.client.class%">
        // ...
        <call method="addSubscriber">
            <argument type="service" id="example.client.curl_auth"/>
        </call>
    </service>

Parameters
----------

The bundle provides parameters for each of the standard Guzzle plugin classes.

- `%guzzle.plugin.async.class%`
- `%guzzle.plugin.backoff.class%`
- `%guzzle.plugin.cache.class%`
- `%guzzle.plugin.cookie.class%`
- `%guzzle.plugin.curl_auth.class%`
- `%guzzle.plugin.history.class%`
- `%guzzle.plugin.log.class%`
- `%guzzle.plugin.md5_validator.class%`
- `%guzzle.plugin.command_content_md5.class%`
- `%guzzle.plugin.mock.class%`
- `%guzzle.plugin.oauth.class%`

Logging
-------

Guzzle clients are automatically connected to your Symfony2 application's Monolog through the `misd_guzzle.log.monolog` service.

Caching
-------

The bundle provides the `misd_guzzle.cache.filesystem` service, which allows you to quickly take advantage of caching (by storing files in your `app/cache` folder). Simply add the service to your client:

    // MyBundle/Resources/config/services.xml

    <service id="example.client" class="%guzzle.client.class%">
        // ...
        <call method="addSubscriber">
            <argument type="service" id="misd_guzzle.cache.filesystem"/>
        </call>
    </service>

This will be slower than using, say, Memcache, but doesn't require any dependencies.
