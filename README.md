# docker.uwsgi-nginx-flask-emperor #

Base on  **[tiangolo/uwsgi-nginx:python2.7](https://github.com/tiangolo/uwsgi-nginx-docker)**, we also installed following software into container for use.

- uwsgi
- nginx
- flask

## Comparison between my repository ##

To find the fittest repository by the following image.


![](git-repository-comparison2.png)

Go to [cutejaneii/docker.uwsgi-nginx-flask-spark](https://github.com/cutejaneii/docker.uwsgi-nginx-flask-spark)

Go to [cutejaneii/spark-streaming-kafka-to-cassandra](https://github.com/cutejaneii/spark-streaming-kafka-to-cassandra)

Go to [cutejaneii/docker.nginx-cassandra-spark-emperor](https://github.com/cutejaneii/docker.nginx-cassandra-spark-emperor)

Go to [cutejaneii/docker.uwsgi-nginx-flask-emperor](https://github.com/cutejaneii/docker.uwsgi-nginx-flask-emperor)

## What this project do ? ##

To make multiple flask applications run at the same time, we modify nginx.conf and uwsgi config, and use **[emperor mode](http://uwsgi-docs.readthedocs.io/en/latest/Emperor.html)** to monitor the folder (/etc/uwsgi/vassals) if any .ini files add/delete/modified.

## Usage ##

To run it if you DO NOT need to volumn folder:

> docker run -d -p 9090:80 --name nginx-uwsgi-emperor cutejaneii/docker.uwsgi-nginx-flask-emperor

To run it if you need your docker container work with data volumes:

> docker run -d -p 9090:80 --name nginx-uwsgi-emperor \
  -v /{host_folder}/nginx.conf:/etc/nginx/conf.d/nginx.conf \
  -v /{host_folder}/app:/app \
  -v /{host_folder}/vassals:/etc/uwsgi/vassals \
  cutejaneii/docker.uwsgi-nginx-flask-emperor

## How to test ? ##

Open following urls from browser:

http://yourIP:9090/test/    -- You will see the cotent: Hello, this is test 1~
 
http://yourIP:9090/test2/   -- You will see the cotent: Hello, this is test 2~

## How to deploy your flask application ? ##

- Create a subfolder and put it into /app folder.
- Put your python files into subfolder which you created.
- Add .ini file for your application and put it into /vassals folder
- Edit nginx.conf

Remeber to RESTART YOUR CONTAINER!
