# Using Domain for Feedbacky Instance

You probably don't want to have your Feedbacky instance running on IP like [http://188.222.111.00:8090](http://188.222.111.00:8090) but on a domain like [https://app.feedbacky.net](https://app.feedbacky.net), here is how to set up domain.

### Domain using Apache VHosts

If you have Apache HTTP server installed you can create Virtual Host and use your domain to access the web app. Create configuration file at `/etc/apache2/sites-available`, it must end with `.conf` extension, for example `feedbacky.domain.conf`.

Your file should contain this:

```text
<VirtualHost *:80>
      ServerName {DOMAIN_NAME}
      ProxyPreserveHost On
      ProxyRequests Off
      ProxyVia Off
      ProxyPass / {SERVER_IP}:{CLIENT_APP_PORT}/
      ProxyPassReverse / {SERVER_IP}:{CLIENT_APP_PORT}/
</VirtualHost>
```

Where **`{DOMAIN_NAME}`** is domain name you want to use eg. feedback.page.net, **`{SERVER_IP}`** is IP of your server and **`{CLIENT_APP_PORT}`** is port of your Feedbacky instance from `.env` file you configured.

By default Proxy is disabled in Apache HTTP Server and you must enable it via `a2enmod proxy` to make it work.

Once you save the file type `a2ensite {FILE_NAME}` eg. `a2ensite feedbacky.domain.conf` and your virtual host should be enabled.

### Configure DNS records to point to your webiste

At your DNS provider just create A record that points to your server IP address and Apache should handle it then.

