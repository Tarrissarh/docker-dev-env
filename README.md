# Development environment

## Commands:

- Start dev env: ```docker-compose up -d```
- Start dev env with rebuild: ```docker-compose up -d --build```
- Watch logs: ```docker-compose logs -f [container_name]```
- Git commands for project:
    - Set email: ```git config --global user.email [user_email]```
    - Set username: ```git config --global user.name [user_name]```
    - Ignore filemode changes:
        - ```git config core.filemode false```
        - ```git config --global core.filemode false```
- Log in container: ```docker exec -it [container_id or container_name] bash [/bin/sh]```

## Containers:

- Nginx (latest alpine)
- PHP (8.0 alpine)
- Mysql 8.0
- MongoDB & Mongo-Express
- Memcached (latest)
- RabbitMQ Management (3.8-management-alpine)

## Urls:

- Default app: `http://localhost:8080`
- Mongo Express: `http://localhost:8081`
- RabbitMQ Management: `http://localhost:15672`