# Loopback API 

> [Loopback.io](https://loopback.io/)

> [MongoDB](https://docs.mongodb.com/)

> src -> [youtube](https://www.youtube.com/watch?v=UTxhKZuVaG8)

> c9.io -> [link](https://ide.c9.io/isphins/loopback-101)

## Node Update

```
$ nvm install 8.3.0
```

## Install Mongodb

> src -> https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

> src c9 -> https://community.c9.io/t/setting-up-mongodb/1717

```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

-- I use Ubuntu 14.04

$ echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

$ sudo apt-get update

$ sudo apt-get install -y mongodb-org

$ mkdir data
$ echo 'mongod --bind_ip=$IP --dbpath=data --nojournal --rest "$@"' > mongod
$ chmod a+x mongod

$ ./mongod
```

## loopback

```
$ npm install -g loopback-cli  

$ lb

> application name : ???
> directory project : ???
> version : 3.x
> application : api-server
```

### install loopback-connector-mongodb

```
$ sudo npm install --save loopback-connector-mongodb
```

### use mongo datasource

```
$ lb datasource mongoDS --connector mongoDB

> MongoDB

> host : localhost
> port : 27017
> user : 
> database : loopback00
```

### Create Model

```
$ lb model meetup

> PersistedModel 
> RESTAPI : Yes
> REST Url :
> server only : common

> Property name : name
> Type : string
> Required? : Yes
> Default value :

> Property name : city
> Type : string
> Required? : Yes
> Default value :

enter for exit create model
```

### Model

> will see model in common folder 

### Start Server

> node .

### Test API

> POST /meetups

![](http://www.clipular.com/c/5083098411958272.png?k=_pacJ2yVzAQlBO4FcAP_f95pk48)


### Mongodb Check

```
$ use loopback00
$ show collections
$ db.meetup.find()

-- { "_id" : ObjectId("59b5407029790131f2332000"), "name" : "vue meetup", "city" : "Miamy" }
```

### Authentication 

```
$ lb acl

> ACL entry to : meetup
> ACL scope : All methods and properties  
> access type : All
> Select the role : Any unauthentication user
> permission to apply : Explicity deny access
```

### Run Server 

> node .

> go POST /Users

![](http://www.clipular.com/c/5182653337960448.png?k=4ShqAWIeyy-UPzGVcMby5hgRabI)

```
{
    "email": "aaa@mail.com",
    "password": "123456789"
}
```

> go POST /Users/login

test fail
```js
{
    "email": "aaa@mail.com",
    "password": "1234"
}
```

```js
{
  "error": {
    "statusCode": 401,
    "name": "Error",
    "message": "login failed",
    "code": "LOGIN_FAILED",
    "stack": "Error: login failed\n   "
    }
}
```

test OK

```
{
  "id": "nOChNHZzW0VZuYZA74Gzw7m261LEzAMOpG85m8BGo4rBdDalBFbIBCtN2FBRBx1o",
  "ttl" : 1209600,
  "created": "2017-09-10T13:56:06.736Z",
  "userId": "59b543bc3056e332494752d9"
}
```

> go SET Access Token paste : nOChNHZzW0VZuYZA74Gzw7m261LEzAMOpG85m8BGo4rBdDalBFbIBCtN2FBRBx1o

> SET Access Token click

> go GET Meetups

> OK

> go POST /Users/logout


## Go models meetup.json

```js
{
  "name": "meetup",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {
    "name": {
      "type": "string",
      "required": true
    },
    "city": {
      "type": "string",
      "required": true
    }
  },
  "validations": [],
  "relations": {},
  "acls": [
    //-- remove this
    /*{
      "accessType": "*",
      "principalType": "ROLE",
      "principalId": "$unauthenticated",
      "permission": "DENY"
    }*/
  ],
  "methods": {}
}

```

## Stop server 

## Type lb

```
$ lb acl

> ACL entry : meetup
> ACL scope : 
> access Type : Write
> role : Any unauthentication user
> permission to apply : Explicity deny access
```

> OK

> GET meetups  

> can get without login

![](http://www.clipular.com/c/6025773134905344.png?k=ZkbGjXxIXKCj23UX25s6OmPjYM4)

> AND When POST meetups must login 

![](http://www.clipular.com/c/4956436336738304.png?k=cEqnx4PMIX2jnbQTv3rLysqFavA)

![](http://www.clipular.com/c/4629087351209984.png?k=bcaH3VBQZRlljEq2X-TePNstHrA)