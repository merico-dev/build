<img src="https://merico-build.s3-us-west-2.amazonaws.com/Merico-Build-logo.svg" height="30"/>

![build passing](https://badgen.net/badge/build/passing/4c1?icon=gitlab)
![license](https://badgen.net/github/license/merico-dev/build)
[![Discord](https://img.shields.io/discord/844603288082186240.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/83rDG6ydVZ)

[Merico Build](<https://merico.build/>)是一个致力于用代码分析与数据挖掘赋能开源开发者，维护者，以及社区的web app。它使用新颖的静态分析来更准确地衡量代码贡献的价值和影响。

- 开源维护者可以用Merico Build的数据指标来跟踪社区的发展, 发掘更多成长中的贡献者。 
- 开源开发者可以用Merico Build自动建立一个可分享且实时更新的开源个人主页，跟踪并展示他们的开源贡献。

**为什么选择Merico Build?**

1) 静态分析驱动的代码贡献分析, 提供更加智能的指标。比如衡量代码量时，我们开发了[ELOC](https://playground.merico.build/)来替换信噪比较低的代码行数(LOC)。
2) 多维度综合考虑贡献，包含代码量，依赖关系，重复代码，测试覆盖度，文档覆盖度，以及未来更多指标。
3) 您的开源贡献会自动转化成可以分享的徽章。

Merico始于一个加州大学伯克利分校计算机系的[研究项目](<https://www2.eecs.berkeley.edu/Pubs/TechRpts/2018/EECS-2018-174.pdf>)。 我们诚挚地邀请您加入我们，一起提高开源的透明度、健康度和可持续性。

**如果您想试用Merico Build, 请访问 https://merico.build/. 如果您想贡献代码, scroll down for instructions to set up for development.**

**如果您想提议新的功能或有任何建议，请创建Github Issue或者加入我们的[Discord](<https://discord.gg/83rDG6ydVZ>)!**

## 摘要

* [**依赖**](#依赖)
* [**安装**](#安装)
    * [**前端安装**](#前端安装)
    * [**后端安装**](#后端安装)
* [**配置**](#配置)
* [**使用**](#使用)
* [**参与贡献**](#参与贡献)
* [**License**](#license)

<br/>

## 依赖

Postgres 12.5
<https://www.postgresql.org/download/>

NodeJS 14.x
<https://nodejs.org/>

<br/>

## 安装

本地运行 Merico Build 需要克隆和配置两个仓库： `build-backend` 和 `build-frontend`。

<br/>

## 前端安装

### 克隆

克隆[前端仓库](<https://github.com/merico-dev/build-frontend>)
```
git clone https://github.com/merico-dev/build-frontend.git
```

### 安装依赖
```
npm i
```
<br/>


## 后端安装

### 克隆

克隆[后端仓库](<https://github.com/merico-dev/build-backend>)
```
git clone https://github.com/merico-dev/build-backend.git
```

### 安装依赖
```
npm i
```

### 初始化子模块
```
git submodule init
git submodule update
```

### 安装 `common-backend` 依赖。
```
cd common-backend
npm i
```

### 创建配置文件

重命名 `config/local.example.js` 为 `config/local.js`
<br/>
#### 配置数据库

依赖: Postgres 12.5

1. 创建数据库
```
psql postgres -c "CREATE DATABASE your_db_name"
psql postgres -c "CREATE SCHEMA your_db_name"
psql postgres -c "CREATE USER merico PASSWORD 'merico'"
psql postgres -c "ALTER USER merico WITH SUPERUSER"
psql postgres -c "GRANT ALL ON ALL TABLES IN SCHEMA your_db_name to merico"
psql postgres -c "GRANT ALL ON ALL TABLES IN SCHEMA public to merico"
psql postgres -c "GRANT ALL ON SCHEMA your_db_name to merico"
```

2. 复制或重命名 `/config/database.example.json` 为 `/config/database.json`

3. 更新 `database.json` 中 `development` 对象的内容:
```
{
  "development": {
    "user": "merico",
    "host": "localhost",
    "database": "your_db_name",
    "password": "merico",
    "port": 5432,
    "dialect": "postgres"
  },
  ...
}
```

4. 建表
```
psql -f db/schema-only.sql -d your_db_name -U merico
psql -f db/badge-types.sql -d your_db_name -U merico
psql -f db/refresh-views.sql -d your_db_name -U merico
```
<br/>

## 配置

运行`build-backend`还需要不少的配置工作，其中大部分都是必须的。这一部分的配置将会修改`config/local.js`文件。
<br />

请注意，这里用到的Analytics Engine, RabbitMQ, Minio是我们为开源开发者提供的**共享资源**，因此你可能会在RabbitMQ里收到其他开发者的任务消息。我们正在做一些准备工作将Analytics Engine部分开源，在此之前我们提供了这些共享资源。
<br />

### GitHub Auth

`Merico Build`需要配置GitHub的OAuth来实现用户认证，并访问用户在Github上的公开代码仓库。

1. 登录Github，在[Developer Settings](https://github.com/settings/developers)页面，使用以下配置[创建一个GitHub OAuth应用](<https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app>)：

Property | Value
------------ | -------------
Application name | `你的github用户名-merico`
Homepage URL | `http://localhost:1337/`
Authorization callback URL | `http://localhost:1337/auth/github/callback`
<br />

2. 应用创建成功之后，在你的Github OAuth应用的设置页面，点击`Generate a new client secret`。

3. 将你的GitHub OAuth应用的`Client ID`和`Client Secret`复制粘贴到`config/local.js`配置文件里的`githubAuth`配置项下面。
<br/>

### GitLab Auth

1. 登录Gitlab，从右上角的用户头像菜单里进入：Preferences > Applications。
2. 创建一个新的应用。
3. 给你的应用起一个喜欢的名字。
4. 将`Redirect URI`设置为`http://localhost:1337/auth/gitlab/callback`。
5. 勾选`read_api`和`read_user`权限，点击`Save application`创建应用。
6. 在创建成功的应用设置页面，将`Application Id`和`Secret`复制粘贴到`config/local.js`配置文件的`gitlabAuth`配置项下面。
<br/>

### Analytics Engine 代码分析引擎

`Merico Build`系统通过gRPC来调用代码分析引擎（AE）提供的代码分析服务。我们正在准备着在近期将AE服务开源，敬请关注！

请将`config/local.js`配置文件中的`CA_GRPC_SERVER`配置项设置为我们提供的共享AE服务地址:

`'52.41.108.240:30006'`

<br/>

### Minio

`Merico Build`使用`Minio`来存储代码分析引擎AE生成的Protocol Buffer格式的分析结果文件，`common-backend`子模块负责读取和解析这些文件，并把数据导入Build系统的数据库，用来展示分析报告。

请将`config/local.js`配置文件的`minio`配置项设置为我们提供的共享Minio服务：

```
minio: {
    endPoint: '52.41.108.240',
    port: 30003,
    useSSL: false,
    accessKey: 'GmXnuzA3a27uO8RU',
    secretKey: 'ahx57GhFwZhEBlNENkz0TgVn',
    bucketAE: 'merico-analysis'
},
```
<br/>

### RabbitMQ

`Merico Build`需要通过`RabbitMQ`获取来自代码分析引擎AE的通知，包括分析进度，任务完成等。

请将`config/local.js`配置文件的`RABBIT_MQ_URL`配置项设置为我们提供的共享RabbitMQ服务：

`'amqp://rabbitmq:9UvTFggbQsGhNx4hrRdA24tx@52.41.108.240:30001/rabbitmq'`

<br/>

### AWS S3

下面的设置需要用到你的AWS账号，如果还没有的话可以到这里[注册一个免费AWS账号](https://portal.aws.amazon.com/billing/signup?refid=em_127222&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)。

1. 使用下图中的访问限制设置[创建一个Bucket](https://www.google.com/search?q=aws+s3+create+bucket&oq=aws+&aqs=chrome.0.69i59l3j0i20i263j69i57j69i60l3.1480j1j4&sourceid=chrome&ie=UTF-8)：

<img src="https://s3-us-west-2.amazonaws.com/cdn.mericobuild.com/public_access.png" width="500" />

2. 在新创建的Bucket中[创建一个文件夹](<https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-folders.html#create-folder>)，名为`badges` 。
3. 创建一个IAM用户，或选择一个已有的IAM用户，给这个用户添加`putObject`权限 (如果只是用来测试，也可以简单粗暴的全选S3权限)。复制IAM用户的`Access Key`个`Secret Access Key`。
4. 将`config/local.js`配置文件中的`AWS_S3`配置项的设置，更新为前面几步中的`bucket name`, `Access Key`, `Secret Access Key`, `Region`。

### AWS SES

* 这个配置不是必须的，不配置AWS SES的情况下`Merico Build`无法发送邮件，但并不会导致出错。如果你需要发送邮件的功能，请继续下面的步骤：

下面的设置需要用到你的AWS账号，如果还没有的话可以到这里[注册一个免费AWS账号](https://portal.aws.amazon.com/billing/signup?refid=em_127222&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)。

1. 打开AWS Simple Email Service的控制台。
2. 验证你的Email域名。
3. 域名验证通过后，点击`SMTP Settings`。
4. 点击`Create my SMTP Credentials`。
5. 复制`Access Key`和`Secret Access Key`。
6. 将上一步复制的密钥对填写到`config/local.js`配置文件的`SES`配置项中。
<br/>

### Encryption Key

将`config/local.js`配置文件中的`ENCRYPTION_KEY`修改为一个随机的密码字符串，`Merico Build`系统中的各种数据加密场景将会用到这个字符串作为密钥。
<br/><br/>

## 使用

### 启动后端服务 build-backend
```
npm run dev
```

### 启动前端服务 build-frontend

```
npm run dev
```

等待前端和后端服务启动完成，你就应该可以在`http://localhost:3000/`看到`Merico Build`系统的首页啦.

接下来，你就可以用你的GitHub账号登录`Merico Build`，授权应用访问你的GitHub，然后就可以提交一个代码仓库开始分析啦。

Note: 由于共享的分析资源有限，建议目前先用来分析一些较小规模的代码库。我们已经限制了每个仓库只分析最后的2500个commits，但对于一些比较大的项目（例如Django），分析任务运行时间还是会接近一天。所以尽量选择要分析的代码库规模在5000个commits以内。

<br/>

### Screenshots

Project Profile             |  My Repositories
:-------------------------:|:-------------------------:
<img src="https://merico-build.s3-us-west-2.amazonaws.com/js-callgraph.png" /> | <img src="https://merico-build.s3-us-west-2.amazonaws.com/my_repos.png" />

<br/> 

## 参与贡献

We welcome and appreciate contributions. Feel free to find your own way and put up a pull request, or join us on [our discord](<https://discord.gg/83rDG6ydVZ>) and we can help. 

<br/>

## License

Build is [Apache 2.0 licensed](<https://github.com/merico-dev/build/blob/master/LICENSE>)
