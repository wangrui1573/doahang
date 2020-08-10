version: '2'

services:
  db:
    image: mysql/mysql-server:5.6
    restart: always
    container_name: "mysql_wsl"
    environment:
      MYSQL_ROOT_PASSWORD: Tym8zrnNRpz4
      MYSQL_DATABASE: webstack
      MYSQL_USER: webstack
      MYSQL_PASSWORD: Xym8zrnNRpz
    command: --default-authentication-plugin=mysql_native_password
    networks:
      - "webstacknet"
  redis:
    image: redis:3
    container_name: "redis_wsl"
    restart: always
    networks:
      - "webstacknet"
  webstack:
    image: realwang/webstack-laravel:v1.2.1
    container_name: "wsl"
    ports:
      - 8000:8000
    depends_on:
      - "db"
      - "redis"
    environment:
      LOGIN_COPTCHA: "false"
      DB_HOST: db
      DB_PORT: 3306
      DB_DATABASE: webstack
      DB_USERNAME: webstack
      DB_PASSWORD: Xym8zrnNRpz
    command: ['/entrypoint.sh','new-server']
    networks:
      - "webstacknet"
networks:
  webstacknet:
    driver: bridge
