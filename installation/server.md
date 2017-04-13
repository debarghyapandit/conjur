---
title: Installation - Server
layout: page
---

The Server utilizes a 2-tier architecture, consisting of a stateless API server
and a Postgresql backend. 

There are several options for running the server.

### Docker Compose

Run the API server and database in linked containers. 
This is the fastest way to get started, if you have Docker running.

1) Create the file `docker-compose.yml`

{% highlight yaml %}
pg:
  image: postgres:9.3

possum:
  image: conjurinc/possum
  command: server
  environment:
    DATABASE_URL: postgres://postgres@pg/postgres
    POSSUM_DATA_KEY:
  links:
  - pg:pg
{% endhighlight %}

2) Create the data key

{% highlight shell %}
$ export POSSUM_DATA_KEY=$(docker-compose run --no-deps --rm \
  possum data-key generate)
{% endhighlight %}

3) Start the server and database

{% highlight shell %}
$ docker-compose up -d 
{% endhighlight %}

### Standalone Docker

Run the server using Docker, and bring your own Postgresql.

{% highlight shell %}
$ export POSSUM_DATA_KEY=$(docker run --rm \
  possum data-key generate)
$ docker run -d \
  -e POSSUM_DATA_KEY \
  -e DATABASE_URL=postgresql:/// <- Provide pg URL here
  -p 80:80 \ <- Provide a port mapping here
  possum server
{% endhighlight %}

### AWS

The Conjur server is available as an AMI. A CloudFormation template is available
which will:

* Launch an RDS-powered Postgresql database.
* Launch a load-balanced pair of API servers.
* Grant the API servers permission to use the database.
* Run a load balancer in front of the API servers.

For production operation, you should perform the additional steps:

* Monitor the API servers.
* Configure the load balancer for SSL / HTTPS.

### Build from source

You can "bring your own" deployment architecture by building and running the
Conjur server from source, aganist a Postgresql database of your choosing.

To proceed with this option, visit [Possum on GitHub](https://github.com/conjurinc/possum).