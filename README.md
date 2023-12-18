# docker-containers

## traefik

docker-compose to run the platform behind a reverse proxy.

## tailoringexpert

This docker-compose is intended to create and run docker containers for a tailoringexpert platform without any tenant installation.
The compose file consists of

- db\
  MariaDB database image
- web\
  Apache httpd web server containing the web part of the platform
- app\
  backend of the platform

Before building the images, you will need to build the package
- [tailoringexpert platform](https://github.com/tailoringexpert/platform) and 
- [tailoringexpert web](https://github.com/tailoringexpert/web) 

and place the generated distribution files in

- tailoringexpert-distribution-*docker.tar.gz -> app/rootfs/app
- tailoringexpert-web-*docker.tar.gz -> web/rootfs/app
