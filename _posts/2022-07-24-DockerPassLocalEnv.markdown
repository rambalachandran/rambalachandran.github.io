---
title:  "Passing Local Enviornment Variables to Docker Build Process and Containers"
image:  /assets/images/blog_posts/docker_logo.png
author: Ram Balachandran
# comments: false
# date:   2020-08-23 08:30:00 +0530
categories:
    - DevOps
    - Docker
layout: post
---

Any data scientist or machine learning interested in developing production deployable models need to understand Docker. Docker is one of the most important tools for DevOps that can help to create and maintain a consistent enviroment across development, staging and production. In particular `docker-compose` can be used to configure and interface applications using multiple docker containers. 

## Passing local environment variables
One of the challenges in using dockers is to pass local environment information to the docker. There are a couple of ways to do this.

1. Using the `.env` file in the same location as `docker-compose`. The variables provided in the `.env` file is used to expand the environment of the `docker-compose` command
2. The `env_file` that is provided in the `docker-compose.yml` file. The variables provided in the `env_file` are injected into the environment of the containers.

## Postgresql Docker Example
To see the difference in action, lets work with an example of a postgresql docker container. The `docker-compose.yml` file for this is given below.

```yml
version: '3.8'

services:
  postgres:
    container_name: postgres_container
    image: postgres
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-admin}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-changemenow}
      POSTGRES_DB: ${POSTGRES_DB:-postgres}
      PGDATA: /data/postgres
    volumes:
      - /home/ram/data/docker_data/postgresql:/data/postgres
    env_file: ./config/postgres.env
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped

networks:
  postgres:
    driver: bridge
```

## Passing username and password variables
In this configuration, the default values are provided for the database configurations. However these values can be over-ridden by passing environment variables. For example,
    - The databaser user is `admin`. However this value can be over-riden by passing the variable `POSTGRES_USER`. Similar over-rides can be done for password and database name.
  
Lets create two files `.env`  with the same following values
```sh
POSTGRES_USER=admin2
POSTGRES_PASSWORD=dontchange
POSTGRES_DB=localpg
```
Now when we try to verify if the variable values are passed to docker-compose using the command
```sh
sudo docker compose convert
```
we get the following output where the postgres variable values are correctly substituted.

```sh
services:
  postgres:
    container_name: postgres_container
    image: postgres
    environment:
      POSTGRES_USER: admin2
      POSTGRES_PASSWORD: dontchange
      POSTGRES_DB: localpg
      PGDATA: /data/postgres
    volumes:
      - /home/ram/data/docker_data/postgresql:/data/postgres
    env_file: ./config/postgres.env
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped

networks:
  postgres:
    driver: bridge
```
However, if we create a file  `./config/postgres.env` with the same values, these variables are not subsituted and we get the following values and we get the default values.

## Passing variable to container environment
Lets say we plan to create a custom folder in the container environment (`/data/customdir`), lets define it in `.env` file
```sh
CDIR=/data/customdir`
```
Now if we start the container (`sudo docker compose up`) and try to find the variable in the container environment with following command
```sh
sudo docker exec -t postgres_container echo $CDIR
```
the variable was not defined and we get an empty value returned. However when the same variable (`CDIR`) is defined through the file `./config/postgres.env`, the echo command in docker environment returns the expected value of /`data/customdir`

## Conclusion
To pass a local environment variable to expand/build the `docker-compose.yml` file, we need to define the variable in the `.env` file. However, to pass the variable to the container environment, we can define it in any custom file (ex: `./config/postgres.env`), but this location needs to be provided under the `env_file` part of the yml file.


<!---
# Multi Line  Equation in MathJax: https://stackoverflow.com/a/21565829/1652217
# How to make Mathjax work with jekyll: 
Copy _layouts/post.html to the working directory (You can get the path from `bundle info <theme-name>` which in this case is `minima`)
Copy the following scriptline into `post.html` and it should work off the box.

```
script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
--->