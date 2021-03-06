FROM python:2.7

# Install uWSGI & flask
RUN pip install uwsgi && \
    pip install flask

# Standard set up Nginx
ENV NGINX_VERSION 1.9.11-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
	&& echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install -y ca-certificates nginx=${NGINX_VERSION} gettext-base \
	&& rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

# Make NGINX run on the foreground
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Remove default configuration from Nginx
RUN rm /etc/nginx/conf.d/default.conf

# Copy the modified Nginx conf
COPY nginx.conf /etc/nginx/conf.d/

# Copy uWSGI emperor.ini to monitor /etc/uwsgi/vassals folder
COPY emperor.ini /etc/uwsgi/

# This dir will be monitored by uwsgi emperor
RUN mkdir /etc/uwsgi/vassals

# Install Supervisord
RUN apt-get update && apt-get install -y supervisor \
&& rm -rf /var/lib/apt/lists/*
# Custom Supervisord config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY ./app /app
COPY ./vassals /etc/uwsgi/vassals

WORKDIR /app

CMD ["/usr/bin/supervisord"]
