# Nest-Summary

## I. Basic concept

### 1. Nest CLI

#### 1.1. Create Project with Nest CLI

```sh
npm install -g nest-cli
nest new project-name
```

- More: [Reference](https://docs.nestjs.com/cli/usages#nest-new)

#### 2.2. Nest Generate

You can use nest cli to generate Service, Module, Controller, ... with supported flag

Example

```sh
nest g module module-name
nest g service module-name/services/serivce-name --no-spec --flat
nest g controller module-name/controllers/serivce-name --no-spec --flat
```

- More: [Reference](https://docs.nestjs.com/cli/usages#nest-generate)

### 2. Life Circle

1. Interceptor
2. Guard
3. Filter
4. Pipe
5. Middleware
6. Controller
7. Service

Controll Flow: Middleware => Guard => Interceptor => Pipes => Controller => Filter

More: [(Ref)](https://viblo.asia/p/cach-request-lifecycle-hoat-dong-trong-nestjs-y3RL1awpLao)

### 3. Exception

- Throw excetion: Nestjs Common have basic Http Exeption:
  There are: HttpException, ForbiddenException,

- Example

```typescript
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

- You can custom http exception. [(Reference)](https://docs.nestjs.com/exception-filters#custom-exceptions)

- You need to filter exception for easy handle error when you code
  [Reference](https://docs.nestjs.com/exception-filters)

### 4. Dependency Injection

Is a Design Pattern use Object Oriented Programing Idea

#### Note: Cicular Dependency

Avoid about Cicular dependency [Reference](https://docs.nestjs.com/fundamentals/circular-dependency/)

### 5. TypeORM (or any ORM library)

- Use TypeORM (or any ORM libarary), you can map to your database, code less...
  [docs link ](https://typeorm.io/)

## II. BASIC CRUD

### 1. Repository Pattern

- If you want create a basic CRUD API, You should apply repository pattern
- Repository Pattern have three layers: Repository layer, Service layer, controller layer.
  [Example](https://www.linkedin.com/pulse/implementing-repository-pattern-nestjs-nadeera-sampath/).

- Note: You can write Basic Repository Interface and Basic Repository Abstract to inherit from it, you can code less. You can reference about my code: [Reference](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/common/base/base.abstract.repository.ts)

### 2. Upload file

- This example I use Cloudinary
  [Example](https://github.com/phong-jack/nest-learn/blob/cloudinary-uploads/src/cloudinary/cloudinary.module.ts)

### 3. Cache

- I write a cache module use cache-manager lib. [Code](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/common/cache/cache-memory.module.ts)

- Cache improves your app performace
- You write CacheInterceptor and apply it

## III. Authentication & Authorization

### 1. Basic Auth

- Basic auth [(Example)](https://github.com/phong-jack/nest-learn/blob/nest-repo-swagger/src/modules/auth/guards/auth.guard.ts) (Not recommend use this)

### 2. JWT Access Token & Refesh Token

- I use passport lirary for support AT and RT
- I create 2 Guard (AT Guard and RT Guard)
- Apply to route or controller (Depends your application logic)
- Code: [link](https://github.com/phong-jack/nest-learn/blob/nest-repo-swagger/src/modules/auth/guards/accessToken.guard.ts)

### 3. 3rd login (use github party)

- PassportJS support 3rd login
- You need install PassportJS and Passport/Github

```sh
npm install @nestjs/passport passport passport-github
npm install --save-dev @types/passport-github

```

- [Example](https://github.com/phong-jack/nest-learn/blob/nest-mail-emitter/src/modules/auth/strategies/github.strategy.ts)

### 4. Role Guard

- Use Role Guard for authorize (between difference role)
- [Example](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/modules/auth/guards/role.guard.ts)

### 5. Casl Ability (Policies Guard)

- Need to use Casl Ability when You want guard route by User id together ( between 2 user same roles )
- [Example](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/modules/casl/guards/policy.guard.ts)

## IV. Docker

### 1. Dockerize app

- Write a dockerfile for your app => image

```dockerfile
# Specify the base image
FROM node:14

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install application dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Specify the command to run the application
CMD ["npm", "start"]
```

### 2. Docker compose

- Run services for your app (Mysql, Redis, Elasticsearch,... )

```dockerfile
version: '3.8'

services:
  # MySQL service
  mysql:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - '3306:3306'
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      PMA_HOST: mysql
      PMA_USER: root
      PMA_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    ports:
      - '8080:80'
  # Redis service
  redis:
    container_name: redis
    image: redis:alpine
    expose:
      - 6379
    ports:
      - '6379:6379'
    restart: unless-stopped
  redis_commander:
    container_name: redis_commander
    image: rediscommander/redis-commander:latest
    environment:
      - REDIS_HOSTS=local:redis:6379
    ports:
      - '8088:8081'
    depends_on:
      - redis
volumes:
  mysql_data:
networks:
  test:
    driver: bridge

```

## V. Event Emitter & Queue

### 1. Event Emitter

- I use event emitter for auth module.
- Explain: when i sign up a accout, the app will send an email to accout's email. If I write mail service into signup => when i signup, i have to wait mail sent => it's not good for perfomance
  => I use event emitter to emit an user signup event, when this event emitter => email service will send an email
- Code: [link](https://github.com/phong-jack/nest-learn/blob/2ee8ed75fec8f45a7aba2f6418bc114a011f53a3/src/modules/auth/listeners/user-created.listener.ts)

### 2. Queue

- I use Queue to sync shop data form an API
- Explain: when this fetch service run, I add a sync shop queue => shop data will sync into my database. This job use a lot of Resource so i use queue to run this
- Code: [link](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/common/fetch/services/fetch.service.ts)

## VI. Websocket & Socket.IO

- I use Websocket and Socket.IO for chatting and notification
- Notification: When i create an order => i will emit an event create order => this websocket listen, create an room and user, shop can chat in this room (This room name created by order id)

- Code: [link](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/common/gateway/event.gateway.ts)

## VII. Database

### 1. Key value pairs

- A database design
- Example:

```typescript
@Entity({ name: "setting" })
export class Setting extends BaseEntity {
  // ...
  key: SETTING_KEY;
  value: string;
  // ...
}
```

- More: My setting module [link](https://github.com/phong-jack/food-ordering-nestjs/tree/master/src/modules/setting)
