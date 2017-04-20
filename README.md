# Laravel Queue Worker

A docker image for working with queues being monitored by supervisor as recommended by laravel.

## Instruction to create docker image

docker build -t IMAGE_NAME .

## Instruction to push image do docker hub

docker login
docker tag IMAGE_NAME $DOCKER_ID_USER/IMAGE_NAME
docker push $DOCKER_ID_USER/IMAGE_NAME

## Image usage

services:
   supervisor:
      image: IMAGE_URL
      volumes:
          - ./:/var/www/app

## Supervisor configuration taken from Laravel official documentation.
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan queue:work sqs --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
user=forge
numprocs=5
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log

numprocs directive will instruct Supervisor to run 5 queue:work processes and monitor all of them, automatically restarting them if they fail.