---
layout: post
title: Importing 150 Millon Rows Into MySQL
author: Zhuyi Xue
tags: MySQL
---

**TL;DR**: This only applies to InnoDB storage engine in MySQL. I only tried it
on MySQL 5.7:18. To speed up the importing/restoring process, consider tuning
the folloing three variables in your configuration (e.g. `my.cnf`). You may need
to restart the server if they were not set to be modified dynamically.

```
innodb_buffer_pool_size
innodb_buffer_pool_instances
innodb_log_file_size
```

**Some readings**: 

1. This [post](and
   https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/)
   explains `innodb_buffer_pool_size` and `innodb_log_file_size`.

1. Read the official
   [doc](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) on
   `innodb_buffer_pool_instances` and InnoDB related startup options and system
   variables in general.

<hr/>

If you have more time to spare, you could read my story:

Recently, I came across a development opportunity (still experimenting stage)
for

1. filling up a new MySQL database with over 150 millions rows, then
2. corrupted it (multiple times unintendedly), then
3. restoring the db. 

### Fill up and corrupt
I have had some experience with noSQL database before, and thought 100m was not
that big. But it turned that it could be a decent size for a relatively more
traditional SQL db like MySQL, and it took me a while to complete the task. I
chose SQL this time because the data is unlikely to grow significantly. So here
I'd like to share what I have learned, esp. on how to do it faster.

I had a Linux box (Centos 6, 32GB RAM, 8 cores) without root access. But
luckily, I have had [Docker](https://www.docker.com/) installed before, so in
order to minimize the hassle for setting up a new MySQL db server, I used the
official [MySQL image](https://hub.docker.com/_/mysql/) at DockerHub. It turns
out to be great. I love Docker, who doesn't!

I used an older Docker version. Since I had no root access, updating might be
inconvenient.

```
Docker version 1.7.1, build 786b29d/1.7.1
```

Its doc is archived at
[https://docs.docker.com/v1.7/](https://docs.docker.com/v1.7/).

Here is the command I used to start the server, assuming the project name is
`abc`, and my current directory is `/path/to/abc`

{% highlight bash %}
docker run \
    -p 127.0.0.1:3306:3306 \
    -v /path/to/abc/mysql-conf:/etc/mysql/conf.d
    -v /path/to/abc:/abc \
    --name abc-mysql \
    -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} \
    -d mysql:5.7.18
{% endhighlight %}

Brief explanation:

* `-p/--publish`: I bind the MySQl port inside the Docker container to that on
  my localhost because I would need to connect to the MySQL server from the
  browser on localhost directly. The format is
  [`ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort | containerPort`](https://docs.docker.com/v1.7/).
  So the above option publish/bind/map (I noticed people used different verbs
  for this action) the port 3306, which is the default port number for MySQL, in
  the container to the same number on the localhost/127.0.0.1.
* first `-v/--volume`: mount the MySQL configuration directory so that the
  server knows how to set some specified variables when starting. This is
  documented on the [page](https://hub.docker.com/_/mysql/) of the MySQL Docker
  image.
* second `-v/--volume`: mount the current project directory (`/path/to/abc`) to
  a root directory (`/abc`) inside the container. This is necessary as my data
  file (CSV format) is located in /abc, and in order to load the data and fill
  the initial database I need to access them inside the container. I could do it
  over the brigenetwork between localhost and container because of the
  [`--secure-file-priv`](https://dev.mysql.com/doc/refman/5.7/en/server-options.html#option_mysqld_secure-file-priv)
  thing. Basically, it made whatever data that need loaded to be from a
  specified directory (`/var/lib/mysql-files` in this case, default). So what I
  did was to
  1. get into the container with `docker exec -it abc bash`,
  2. copy files over with `cp -v /abc/*.csv /var/lib/mysql-files/`, I guess you
     might be able to work around by mounting `/path/to/abc` directly to
     `/var/lib/mysql-files`, too, but I haven't tried this.
  3. then connect the server (still inside the container) and import the data
     with `LOAD DATA INFILE...` (detailed later). I could not do the operations
     on localhost because
     [for this particular version of Docker](http://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container),
     seemingly it's only possible to copy file from container to localhost, but
     not the other way around. Such restricts is gone in Docker 1.8.
* `--name`: name of the container
* `-e/--env`: set environment variables needed for setting up the MySQL root
  password inside the container. The way it worked was: 1) I set the same
  environmental variable `MYSQL_ROOT_PASSWORD` on my localhost to be whatever it
  is (secret). 2) when `docker run ...`, bash evaluated `${MY_SQL_ROOT_PASSORD}`
  to be the secret string, and then passed it to the container with `-e`.
* `-d/--detach`: run container in the background
* `mysql:5.7.18`: I nailed it to a specific version for reproducibility


To start the import, I first prepared the data in a csv file without header,
then used a command like below.

{% highlight sql %}
LOAD DATA INFILE 'filename.csv' 
    INTO TABLE table_name 
    FIELDS TERMINATED BY ',' 
    LINES TERMINATED BY '\n'
    (field1, field2, ...);
{% endhighlight %}

`LOAD DATA` is the fastest way I found to fill up the data as I found out. I
also tried ORM with `bulk_create` as in
[Django](https://dev.mysql.com/doc/refman/5.7/en/load-data.html). `LOAD DATA` is
roughly at least 3x faster. If you google around, that's normally what other
people say, as well. For a comprehensive expalanation of the syntax, see the
[doc](https://dev.mysql.com/doc/refman/5.7/en/load-data.html). Meanwhile, there
are other equally or more effective configuration on the database server side
that matter even more, read on.

When I was trying with different ways of importing, I corrupted the database
many times. Sometimes, even after hours of importing, some of my random poking
made the db seemingly freezing. Being impatient, my quick and dirty way to
reinitialize the database is just to remove the Docker container forcefully
(`docker rm -f abc-mysql`), and the restart a new container. This is perhaps
another big advantage of Docker, which enables fast iteration. I am not sure if
it would be as easy if the MySQl server was setup on the localhost directly.

Anyway, my first successful import took over 10 hours, unacceptably long! The
only configuration I did was

{% highlight sql %}
SET GLOBAL INNODB_BUFFER_POOL_SIZE=16G;
{% endhighlight %}

, which turned out be insufficient.

Having corrupted the db so many times, I learned to back it up right away with

{% highlight bash %}
mysqldump -h 127.0.0.1 -u root -p abc > dump_abc.sql
{% endhighlight %}

Unsurprisingly, during the next few operations, being impatient (`SELECT COUNT
(*)` took over 30s!), I corrupted it again, and removed the db entirely.

### Restore

Now, I thought since I've backed it up already, restoring should be simple, but
I didn't want to wait for 10 hours again. The restoring command was

{% highlight bash %}
pv dump_abc.sql.gz | gunzip | mysql -h 127.0.0.1 -u root -p abc
{% endhighlight %}

Yes, I learned this very useful
[Pipe Viewer (`pv`)](http://www.ivarch.com/programs/pv.shtml) to show the
progress. But still, seeing the remaining time is over 10 hr was stressful, and
for some reason, it kept going up gradually.... This time, I calmed down and
read, 

* first, [this post](
https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/)
from [Percona](https://www.percona.com/), and
* then a few chapters from the book,
[High Performance MySQL, 3rd Edition](http://shop.oreilly.com/product/0636920022343.do).
Chapter 1 on MySQl architecture and Chapter 8 on optimizing server settings,
esp. the InnoDB part was very helpful in understanding how things work.

A little reading paid off, I ended up with the following configuration file

{% highlight default %}
[mysqld]
# because: [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. 
# Please use --explicit_defaults_for_timestamp server option
# (see documentation for more details).
explicit_defaults_for_timestamp = 1

innodb_buffer_pool_size = 20G
innodb_buffer_pool_instances = 32
innodb_log_file_size = 16G

max_connections = 500
{% endhighlight %}

and the restoration took about 1.5hr, a nearly 10x reduction, which I was
satisfied at the time. Several side notes:

1. I found that of the three InnoDB related variables, only
   `innodb_buffer_pool_size` could be changed dynamically when the server is on.
   e.g. `SET GLOBAL INNODB_BUFFER_POOL_SIZE=20G;`. To change
   `innodb_buffer_pool_instances` and `innodb_log_file_size`, server needs
   restarted with. Compared to my first import setting, which only modified
   `GLOBAL INNODB_BUFFER_POOL_SIZE`, it turns out that increasing
   `INNODB_BUFFER_POOL_INSTANCES` accordingly is very important, too. (TODO:
   update what the variables mean)
1. `explicit_defaults_for_timestamp` and `max_connections` probably won't affect
   the restoration performance much.


I am pretty sure there are better ways to speed things up, if you feel like
commenting, please let me know.
