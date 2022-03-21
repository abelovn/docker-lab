#!/bin/sh
# docker-lab
# FIRST TASK
docker build -t fontend -f ./lab1/frontend/Dockerfile   ./lab1/frontend/ 

docker run -d -p 80:80 --name my_front my_app:my_app

# SECOND TASK
docker network create backend-net

docker run --name database --network=backend-net \
  -p 5432:5432 \
  -e POSTGRES_USER=django \
  -e POSTGRES_PASSWORD=django \
  -e POSTGRES_DB=django \
  -e PGDATA=/var/lib/postgresql/data/pgdata \
  -d \
  -v /database:/var/lib/postgresql/data \
  postgres:14

docker build   -t backend_app   -f ./lab2/backend/Dockerfile   ./lab2/backend/

docker run --name backend --network=backend-net \
  -p 8000:3000 \
  -d \
  -it \
  backend_app
  
  

  
docker cp patch.data $(docker ps | grep "0.0.0.0:8000->3000/tcp" | awk {'print$1'}):/backend/patch.sh

docker exec $(docker ps | grep "0.0.0.0:8000->3000/tcp" | awk {'print$1'}) /bin/sh /backend/patch.sh

docker exec $(docker ps | grep "0.0.0.0:8000->3000/tcp" | awk {'print$1'}) python manage.py migrate

docker exec -it backend python manage.py createsuperuser

