---
layout: single
title:  "Writing SQL for an unfamiliar PHP application"
date:   2019-09-21 10:33:12 +0100
excerpt: "Dealing with a legacy codebase also means diving into a PHP application"
categories: [tech]
tags: []
---
As I'm currently working for a famous yellow telco with a huge legacy, I recently had to deal with a PHP application. Although there is a PHP dedicated team, we were ~~forced~~ asked to deal with the PHP part as well.

I did have some past experiences with PHP (and to some extend Drupal), so I faced this old dud. Luckily I just needed to spin up a Vagrant box so that was quite easy.

My mission was to add a product. There was a admin page where you could do this manually, but for CI/CD purposes we really needed the SQL. So how to tackle this when you application/database schema and business knowledge is limited?

You can do it the hard way and study and annoy your collegues with a lot of questions. Or you think for a while and you come up with a more creative solution, which was my approach.

MySQL offers a a feature called general log. As the name implies, it logs every single requested query. So I turned it on, added the product using the administrative interface and scraped the queries from the log. I cleaned the queries because all primary and foreign keys where hardcoded, and that's it - job well done.

For completeness, I add the configuration you need to enable general logging in MySQL.

* Create your log file, eg `touch /tmp/mysql.log`
* Open the configuration file, usually called `my.cnf`
* Add the following lines to the `[mysqld]` section
  ```
  general_log = on
  general_log_file = /tmp/mysql.log
  ```
* Restart MySQL
