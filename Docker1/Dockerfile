FROM ubuntu

RUN apt-get update
RUN apt-get install -y nginx

COPY index.html /usr/share/nginx/html/

RUN echo "TEST"

ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
