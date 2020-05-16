# Docker Compose Spring-Boot + MySQL

## Description
- Use `amarnath510/spring-boot-mysql-app` and `amarnath510/mysql-5.6-docker-image` images
- Docker-Compose will create "spring_mysql_ntw" if not present
- Both of the services should be in the same network else Spring app can't connect to MySQL container => App start up will fail
- Depends on (depends_on) doesn't work. I just used it as a documentation to know Spring app is depending on MySQL
- We need "restart" property because Spring application will look for MySQL but MySQL will not be ready immediately so Spring application start up will fail. In order to fix this we added values "on-failure" which will make sure to start app again if failed

## Run
- `$ cd to compose-project`
- `$ docker-compose config` (Always validate when ever you change the compose file)
- `$ docker-compose up -d` (If you want to see logs then remove `-d`)
- `$ docker ps`   (If everything is smooth then both containers should be up and running)

## Test
  - ### Api
  - **POST**: `http://localhost:8200/company/employees/all` and give raw+json input,
    ```
    [
      {
        "firstName": "amarnath",
        "lastName": "chandana",
        "email": "amarnath@docker.com"
      },
      {
        "firstName": "sushma",
        "lastName": "chandana",
        "email": "sushma@docker.com"
      }
    ]
    ```

    OR 
    
  - curl -i -X POST -H  'Content-Type: application/json' -d '[{ "firstName": "amarnath", "lastName": "chandana", "email": "amarnath@docker.com" }]' http://localhost:8200/company/employees/all
    
  - **GET**: `http://localhost:8200/company/employees` to get above results
  - ### MySQL
  - `$ docker container exec -it database bash` (database is the name of the mysql container)
  - `$# mysql -uroot -pdev`
  - `mysql> use company;`
  - `mysql> select * from employees;`
