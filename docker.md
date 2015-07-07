<!-- TOC depth:6 withLinks:1 updateOnSave:1 -->
- [Docker deployment](#docker-deployment)
	- [Update Docker deployment](#update-docker-deployment)
- [Docker build image from Dockerfile](#docker-build-image-from-dockerfile)
- [Docker run image](#docker-run-image)
- [Docker debug commands](#docker-debug-commands)
- [Export and convert Docker image](#export-and-convert-docker-image)
<!-- /TOC -->
****************************************

# Docker deployment
  wget -qO- https://get.docker.com/ | sh
  docker run hello-world

## Update Docker deployment
  wget -N https://get.docker.com/ | sh

# Docker build image from Dockerfile
  docker build --no-cache -t lad/transmart .

# Docker run image
  docker run -d -p 80:8080 -p 5432:5432 -p 8983:8983 lad/transmart --name transmart

# Docker debug commands
  docker exec kickass_newton ps axuf

# Export and convert Docker image
  docker export -o etriks 38c2887c44f6
  qemu-img convert -c -p -O qcow2 etriks-transmart-docker-v1.tar etriks-transmart-docker-v1.qcow2
