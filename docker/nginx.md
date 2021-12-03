nginx 設定



nginxコンテナで受けたリクエストをclientやapiに振り分ける際に必要なこと：
https://qiita.com/nagi125/items/09ddbbfa923c0999494e


nginx -- nuxtjs をつなぐ設定のポイント:
```
proxy_pass  http://{docker-composeで定めたサービス}:3000
```

```yml
# docker-compose.yml

version: '3.8'
services:
  frontend:
    container_name: front_end
    build: frontend
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/app

  nginx:
    container_name: nginx
    build: ./nginx
    ports:
      - 80:80
      - 443:443
    depends_on:
      - frontend
```

```conf
# nginx.conf
events{
}
http{
    server {
        listen 80 default;
        server_name localhost; 
        location / {
            proxy_pass  http://frontend:3000;
        }
    }
    server {
        listen 443 ssl;
        server_name localhost;
        ssl_certificate /etc/certs/localhost.pem;
        ssl_certificate_key /etc/certs/localhost-key.pem; 
        location / {
            proxy_pass  http://frontend:3000;
        }
    }
}
```
