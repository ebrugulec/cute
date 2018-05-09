# Cute :relaxed:
---

Project management application like Trello. 

This project has used **Ruby on Rails** for the back-end, **VueJs** for the front-end and for the database **PostgreSQL**

### Installiation
---
> for Linux
- we need to database. Install PostgreSQL last version with this command:
```shell
  $ sudo apt-get install postgresql-9.4 postgresql-client-9.4
```

- To check that the PostgreSQL server was correctly installed and is running, you can use the command ps:
```shell
$ ps -ef | grep postgre
```

- Now we can install **cute** application. You can clone this repo with command:
```shell
$ git clone git@github.com:ebrugulec/cute.git
```

- we need to install the gems required for this application:
```shell
$ cd cute
$ bundle
```

- We must setup database:
```shell
$ rake db:create
$ rake db:migrate
```

- Now, We can run application.
> But you have two options here. You can start Rails and WebPack server separately.
```shell
$ rails server
$ ./bin/webpack-dev-server
```
> or You can start both with foreman. <a href="https://github.com/ddollar/foreman" target="_blank">**Foreman**</a> has to be installed on your computer.
```shell
$ foreman start -f Procfile.dev -p 3000
```
