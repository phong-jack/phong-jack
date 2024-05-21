# Nest-Summary

## I. Basic concept

### Nest CLI

1. Create Project with Nest CLI

```sh
npm install -g nest-cli
nest new project-name
```

[Reference](https://docs.nestjs.com/cli/usages#nest-new)

2. Nest Generate

You can use nest cli to generate Service, Module, Controller, ... with supported flag

Example

```sh
nest g module module-name
nest g service module-name/services/serivce-name --no-spec --flat
nest g controller module-name/controllers/serivce-name --no-spec --flat
```

[Reference](https://docs.nestjs.com/cli/usages#nest-generate)

### Life Circle

1. Interceptor
2. Guard
3. Filter
4. Pipes
5. Middleware
6. Controller
7. Service

Controll Flow: Middleware => Guard => Interceptor => Pipes => Controller => Filter

More in: [(Ref)](https://viblo.asia/p/cach-request-lifecycle-hoat-dong-trong-nestjs-y3RL1awpLao)

### Exception

throw excetion: Nestjs Common have basic Http Exeption:
There are: HttpException, ForbiddenException,

Example

```typescript
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

You can custom http exception. [(Reference)](https://docs.nestjs.com/exception-filters#custom-exceptions)

You need to filter exception for easy handle error when you code
[Reference](https://docs.nestjs.com/exception-filters)

### Dependency Injection

Is a Design Pattern use Object Oriented Programing Idea

#### Note: Cicular Dependency

Avoid about Cicular dependency [Reference](https://docs.nestjs.com/fundamentals/circular-dependency/)

### TypeORM (or any ORM library)

- Use TypeORM (or any ORM libarary), you can map to your's database, code less...
  [docs link ](https://typeorm.io/)

## II. BASIC CRUD

### Repository Pattern

- If you want create a basic CRUD API, You should apply repository pattern
- Repository Pattern have three layers: Repository layer, Service layer, controller layer.
  [Example](https://www.linkedin.com/pulse/implementing-repository-pattern-nestjs-nadeera-sampath/).

- Note: You can write Basic Repository Interface and Basic Repository Abstract to inherit from it, you can code less. You can reference about my code: [Reference](https://github.com/phong-jack/food-ordering-nestjs/blob/master/src/common/base/base.abstract.repository.ts)

### Upload file

- This Example I use Cloudinary
  [Example](https://github.com/phong-jack/nest-learn/blob/cloudinary-uploads/src/cloudinary/cloudinary.module.ts)

### Cache

## III. Authentication & Authorization

### 1. Basic JWT

### 2. JWT Access Token & Refesh Token

### 3. 3rd login (use github party)

### 4. Role Guard

### 5. Casl Ability (Policies Guard)

##

## Docker
