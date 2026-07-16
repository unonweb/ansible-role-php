NOTES
=====

PHP-FPM
-------

- FastCGI Process Manager
- Alternative to the traditional PHP-CGI (Common Gateway Interface)
- Focus: improve performance of PHP-based applications
- Works as a process manager, managing PHP processes and handling PHP requests separately from the web server. 
- By doing so, it can efficiently handle multiple PHP requests concurrently, leading to a significant reduction in latency and improved overall performance.

### How it works
Operates in tandem with the web server.
When a PHP request is received, the web server forwards it to the PHP-FPM process manager, which then handles the request via a pool of child processes. These child processes are separate instances of PHP, each capable of handling individual requests independently.

CONFIG
======

```sh

```

EXAMPLES
========

Nextcloud
---------

### install

```yml
    # php
    - ansible.builtin.import_role:
        name: php
      vars:
        php_version: 8.4
        php_install_packages:
        - php{{ php_version }}-gmp
        - php{{ php_version }}-bcmath
        - php{{ php_version }}-gd
        - php{{ php_version }}-mysql
        - php{{ php_version }}-curl
        - php{{ php_version }}-mbstring
        - php{{ php_version }}-intl
        - php{{ php_version }}-imagick
        - php{{ php_version }}-xml
        - php{{ php_version }}-zip
        - php{{ php_version }}-fpm
        php_fpm_config_vars:
        - { key: 'memory_limit', value: "1024M" }
        - { key: 'upload_max_filesize', value: "1G" }
        - { key: 'max_execution_time', value: 30 }
        - { key: 'date.timezone', value: "Europe/Berlin" }
        # https://docs.nextcloud.com/server/stable/admin_manual/installation/server_tuning.html#comments
        - { key: 'opcache.save_comments', value: 1 }
        # https://www.scalingphpbook.com/blog/2014/02/14/best-zend-opcache-settings.html
        # https://docs.nextcloud.com/server/stable/admin_manual/installation/server_tuning.html#revalidation
        - { key: 'opcache.enable', value: 1 }
        - { key: 'opcache.interned_strings_buffer', value: 16 }
        - { key: 'opcache.max_accelerated_files', value: 7000 }
        - { key: 'opcache.memory_consumption', value: 128 }
        - { key: 'opcache.revalidate_freq', value: 0 }
        - { key: 'opcache.validate_timestamps', value: 0 }
        - { key: 'opcache.fast_shutdown', value: 1 }
        - { key: 'apc.enable_cli', value: 1 }
```

### config

- https://docs.nextcloud.com/server/stable/admin_manual/installation/php_configuration.html
- https://www.scalingphpbook.com/blog/2014/02/14/best-zend-opcache-settings.html