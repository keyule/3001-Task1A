# TIC3001 Task 1A
- Name: Ke Yule
- Student Number: A0211495H E0493826
- Github: 

### Task 1.1
#### Instructions
1. Create docker file at /app/Dockerfile containing:

```yaml
# syntax=docker/dockerfile:1
FROM node:18
ENV NODE_ENV=production

WORKDIR /app

COPY package*.json .
COPY package-lock.json* .

RUN npm install --production

COPY . .

CMD [ "npm", "start" ]
```
2. `docker build -t <AppName> . `
3. `docker run -p 3000:3000 <AppName>`

#### Screenshot 

![Task 1A 1.1](#placeholder)


### Task 1.2
#### Instructions

1. Create a nginx.conf file containing:

```json
server {
    listen 9001;
    server_name localhost;

    location / {
        proxy_pass http://localhost:80;
    }
}
```
2. Create a docker file containing:

```yaml
FROM nginx:latest
COPY . /usr/share/nginx/html/

COPY nginx.conf /etc/nginx/conf.d/
```

3. `docker build -t <NginxAppName> . `
4. `docker run -p 9001:9001 <NginxAppName> `

#### Screenshot

![Task 1A 1.2](#placeholder)

### Task 1.3
#### Instructions

1. Reuse the same Dockerfile for the node app
2. Create a nginx.conf file in the nginx folder containing: 

```json 
events {}
http {
  server {
    listen 80;
    location / {
      proxy_pass http://web:3000;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
}
```
3. Create a docker-compose.yml in the root folder containing: 
```yaml
version: '3.3'
services:
  web:
    build: ./app
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - web
```

4. `docker-compose up --build`

*Note: I changed the port to port 3000 in index.js*

#### Sreenshot

![Task 1A 1.3](#placeholder)