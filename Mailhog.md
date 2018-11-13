# Mailhog



1. install 

    `brew install mailhog`

2. Add service

    ```yml
    proxy:
      mailhog:
       - mail.dogme.lndo.site
    
    services:
      mailhog:
        type: mailhog
          hogfrom:
             - appserver
    tooling:
      sendmail:
        service: appserver
        cmd: /usr/local/bin/mhsendmail
      alert:
        service: appserver
        cmd: php /app/mail.php
        description: Tell Leia something important
    ```

3. Rebuild 

`lando rebuild -y`

4. Install Drupal Module SMTP Authentication Support

    `composer require drupal/smtp`

5. Enable SMTP Authentication Support

6. In the module SMTP Authentication Support, **Install Option** must be **off**

7. Clear cache

    `lando drush cr`

8. Refresh your web site and access to Mailhog by **mail.dogme.lndo.site**



From:

* https://docs.devwithlando.io/services/mailhog.html

*  https://www.drupaleasy.com/blogs/ultimike/2018/01/testing-local-drupal-site-emails-lando-and-mailhog