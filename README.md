# ServerPilot Core API v1.0

The ServerPilot API is RESTful and allows you to manage ServerPilot resources
using HTTP requests. All responses return JSON objects, including errors.

## Authentication

The ServerPilot API supports authentication through Basic Auth. You must provide
your `client_id` and API `key`. All requests require authentication.

You can manage your API credentials from your
[account](https://manage.serverpilot.io/#account).

Only one API key is active at a time. Take care to keep your API key secret!


## Base URL

To ensure data privacy, all API requests must be made over HTTPS. Unencrypted
requests made over plain HTTP are not supported.

```
https://api.serverpilot.io/v1
```

<!--
## Summary of Resource URL Patterns
```
/servers
/servers/:id
/sysusers
/sysusers/:id
/apps
/apps/:id
/apps/:id/ssl
/dbs
/dbs/:id
/actions/:id
```
-->

# Resources & Methods

**Servers**

  * [List all Servers](#list-all-servers)
  * [Create a Server](#create-a-server)
  * [Retrieve an existing Server](#retrieve-an-existing-server)
  * [Delete a Server](#delete-a-server)
  * [Update a Server](#update-a-server)

**System Users**

  * [List all System Users](#list-all-system-users)
  * [Create a System User](#create-a-system-user)
  * [Retrieve an existing System User](#retrieve-an-existing-system-user)
  * [Delete a System User](#delete-a-system-user)
  * [Update a System User](#update-a-system-user)

**Apps**

  * [List all Apps](#list-all-apps)
  * [Create an App](#create-an-app)
  * [Retrieve an existing App](#retrieve-an-existing-app)
  * [Delete an App](#delete-an-app)
  * [Update an App](#update-an-app)
  * [Add a SSL Cert](#add-a-ssl-cert)
  * [Delete SSL for an App](#delete-ssl-for-an-app)

**Databases**

  * [List all Databases](#list-all-databases)
  * [Create a Database](#create-a-database)
  * [Retrieve an existing Database](#retrieve-an-existing-database)
  * [Delete a Database](#delete-a-database)
  * [Update the Database User Password](#update-the-database-user-password)

**Actions**

  * [Check status of an Action](#check-status-of-an-action)

## Servers

Servers are Ubuntu 12.04 or 14.04 machines that are managed by ServerPilot.

Every App, Database, and System User is related to a Server. By default, all
Servers will have a `serverpilot` System User.

### List all Servers
```GET /servers```

```
$ curl https://api.serverpilot.io/v1/servers \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data": [
  {
    "id": "FqHWrrcUfRI18F0l",
    "name": "www1",
    "autoupdates": true,
    "firewall": true,
    "lastaddress": "1.2.3.4",
    "lastconn": 1403130552,
    "datecreated": 1403130551
  }, {
    "id": "4zGDDO2xg30yEeum",
    "name": "vagrant",
    "autoupdates": true,
    "firewall": true,
    "lastaddress": "1.2.3.4",
    "lastconn": 1403130554,
    "datecreated": 1403130553
  }]
}
```

### Create a Server
```POST /servers```

| Name   | Type     | Description
| ------ | :------: | :---------------------------------------:
| `name` | `string` | **Required**. The nickname of the Server.

```
$ curl https://api.serverpilot.io/v1/servers \
   -u {CLIENTID}:{APIKEY} \
   -H "Content-Type: application/json" \
   -d '{"name": "www2"}'
```

```json
{
  "actionid": "tW2fu4hjHnsix6Rn",
  "data":
  {
    "id": "UXOSIYrdtL4cSGp3",
    "name": "www2",
    "autoupdates": true,
    "firewall": true,
    "lastaddress": null,
    "lastconn": null,
    "datecreated": 1403130553,
    "apikey": "nqXUevYSkpW09YKy7CY7PdnL14Q1HIlAfniJZwzjqNQ",
  }
}
```

### Retrieve an existing Server
```GET /servers/:id```

```
$ curl https://api.serverpilot.io/v1/servers/UXOSIYrdtL4cSGp3 \
   -u {CLIENTID}:{APIKEY} \
```

```json
{
  "data":
  {
    "id": "UXOSIYrdtL4cSGp3",
    "name": "www2",
    "autoupdates": true,
    "firewall": true,
    "lastaddress": "1.2.3.4",
    "lastconn": 1403130554,
    "datecreated": 1403130553
  }
}
```

### Delete a Server
```DELETE /servers/:id```

```
$ curl https://api.serverpilot.io/v1/servers/4zGDDO2xg30yEeum \
   -u {CLIENTID}:{APIKEY} \
   -X DELETE
```

```json
{
  "data": {}
}
```

### Update a Server
```POST /servers/:id```

| Name          | Type     | Description
| ------------- | :------: | :---------------------------------------
| `firewall`    | `bool`   | Describes the "enabled" state of the Server firewall. `false` means the firewall is not enabled.
| `autoupdates` | `bool`   | Describes the "enabled" state of automatic system updates. `false` means automatic system updates are not enabled,

```
$ curl https://api.serverpilot.io/v1/servers/UXOSIYrdtL4cSGp3 \
   -u {CLIENTID}:{APIKEY} \
   -d '{"firewall": false}'
```

```json
{
  "actionid": "g3kiiYzxPgAjbwcY",
  "data":
  {
    "id": "UXOSIYrdtL4cSGp3",
    "name": "www2",
    "autoupdates": true,
    "firewall": false,
    "lastaddress": "1.2.3.4",
    "lastconn": 1403130554,
    "datecreated": 1403130553
  }
}
```

## System Users

System Users are Linux user accounts that ServerPilot creates on your Server.
Every App belongs to and runs as one of these System Users. You can log in to
your Server as a System User via SSH to deploy an App or view an App's log
files.

By default, ServerPilot creates the `serverpilot` System User.

The home directory of each System User created by ServerPilot is

```
/srv/users/USERNAME
```

Under the System User's home directory are additional directories:

  * `apps` — each app has its own directory under here.
  * `log` — each app has its log files here.

### List all System Users
```GET /sysusers```

```
$ curl https://api.serverpilot.io/v1/sysusers \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data": [
  {
    "id": "PdmHhsb3fnaZ2r5f",
    "name": "serverpilot",
    "serverid": "FqHWrrcUfRI18F0l",
  },
  {
    "id": "RvnwAIfuENyjUVnl",
    "name": "serverpilot",
    "serverid": "4zGDDO2xg30yEeum",
  }]
}
```

### Create a System User
```POST /sysusers```

| Name       | Type     | Description
| ---------- | :------: | :---------------------------------------
| `serverid` | `string` | **Required**. The id of the Server.
| `name`     | `string` | **Required**. The name of the System User.
| `password` | `string` | The password of the System User. If user has no password, they will not be able to log in.

```
$ curl https://api.serverpilot.io/v1/sysusers \
   -u {CLIENTID}:{APIKEY} \
   -H "Content-Type: application/json" \
   -d '{"serverid": "FqHWrrcUfRI18F0l", \
        "name": "derek", \
        "password": "hlZkUk"}'
```

```json
{
  "actionid": "nnpgQoNzSK11fuTe",
  "data":
  {
    "id": "PPkfc1NECzvwiEBI",
    "name": "derek",
    "serverid": "FqHWrrcUfRI18F0l",
  }
}
```

### Retrieve an existing System User
```GET /sysusers/:id```

```
$ curl https://api.serverpilot.io/v1/sysusers/PPkfc1NECzvwiEBI \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data":
  {
    "id": "PPkfc1NECzvwiEBI",
    "name": "derek",
    "serverid": "FqHWrrcUfRI18F0l",
  }
}
```

### Delete a System User
```DELETE /sysusers/:id```

**Warning**: Deleting a System User will delete all Apps (and Databases)
associated.

```
$ curl https://api.serverpilot.io/v1/sysusers/PPkfc1NECzvwiEBI \
   -u {CLIENTID}:{APIKEY} \
   -X DELETE
```

```json
{
  "actionid": "9tvygrrXZulYuizz",
  "data": {}
}
```

### Update a System User
```POST /sysusers/:id```


| Name       | Type     | Description
| ---------- | :------: | :----------
| `password` | `string` | **Required**. The new password of the App user.

```
$ curl https://api.serverpilot.io/v1/sysusers/RvnwAIfuENyjUVnl \
   -u {CLIENTID}:{APIKEY} \
   -d '{"password": "mRak7S"}'
```

```json
{
  "actionid": "OF42xCWkKcaX3qG2",
  "data":
  {
    "id": "RvnwAIfuENyjUVnl",
    "name": "serverpilot",
    "serverid": "4zGDDO2xg30yEeum",
  }
}
```

## Apps

Apps are your web applications. Sometimes people call apps "websites".

ServerPilot currently supports PHP 5.4 and 5.5 apps.

ServerPilot configures your servers with Nginx as the public-facing webserver.
Nginx serves static files and all other requests are proxied to Apache 2.4 so
that you can use .htaccess files. PHP is configured to run via FPM. Each app
can have multiple MySQL databases.

The web root directory for each app is:

```
/srv/users/serverpilot/apps/{APPNAME}/public
```

ServerPilot does not manage DNS for you. In order for you to access your apps
by their domain name, you must make the appropriate changes in your domain's
DNS zone so that your domain name resolves to your server's IP address. You can
do this where you currently manage DNS for your domain.

### List all Apps
```GET /apps```

```
$ curl https://api.serverpilot.io/v1/apps \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data": [
  {
    "id": "c77JD4gZooGjrF8K",
    "name": "blog",
    "sysuserid": "RvnwAIfuENyjUVnl",
    "domains": ["www.myblog.com", "blog.com"],
    "ssl": null,
    "serverid": "4zGDDO2xg30yEeum",
    "runtime": "php5.4",
  },
  {
    "id": "B1w7yc1tfUPQLIKS",
    "name": "store",
    "sysuserid": "RvnwAIfuENyjUVnl",
    "domains": ["www.mystore.com", "mystore.com"],
    "ssl": {
      "key": "-----BEGIN PRIVATE KEY----- ...",
      "cert": "-----BEGIN CERTIFICATE----- ...",
      "cacerts": "-----BEGIN CERTIFICATE----- ...",
    },
    "serverid": "4zGDDO2xg30yEeum",
    "runtime": "php5.4",
  }]
}
```

### Create an App
```POST /apps```

| Name        | Type           | Description
| ----------- | :------------: | :---------------------------------------
| `name`      | `string`       | **Required**. The nickname of the App.
| `sysuserid` | `string`       | **Required**. The System User that will "own" this App. Since every System User is specific to a Server, this implicitly determines on which Server the App will be created.
| `runtime`   | `string`       | **Required**. The PHP runtime for an App. Choose from `php5.4` or `php5.5`.
| `domains`   | `array`        | An array of domains that will be used in the webserver's configuration. If you set your app's domain name to *example.com*, Nginx and Apache will be configured to listen for both *example.com* and *www.example.com*. **Note**: The complete list of domains must be included in every update to this field.

```
$ curl https://api.serverpilot.io/v1/apps \
   -u {CLIENTID}:{APIKEY} \
   -H "Content-Type: application/json" \
   -d '{ \
          "name": "gallery", \
          "sysuserid": "RvnwAIfuENyjUVnl", \
          "domains": ["myartgallery.com", "www.myartgallery.com"], \
       }'
```

```json
{
  "actionid": "dIrCNoWunW92lPjw",
  "data":
  {
    "id": "nlcN0TwdZAyNEgdp",
    "name": "gallery",
    "sysuserid": "RvnwAIfuENyjUVnl",
    "domains": ["www.myartgallery.com", "myartgallery.com"],
    "ssl": null,
    "serverid": "4zGDDO2xg30yEeum",
    "runtime": "php5.4",
  }
}
```

### Retrieve an existing App
```GET /apps/:id```

```
$ curl https://api.serverpilot.io/v1/apps/nlcN0TwdZAyNEgdp \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data":
  {
    "id": "nlcN0TwdZAyNEgdp",
    "name": "gallery",
    "sysuserid": "RvnwAIfuENyjUVnl",
    "domains": ["www.myartgallery.com", "myartgallery.com"],
    "ssl": null,
    "serverid": "4zGDDO2xg30yEeum",
    "runtime": "php5.4",
  }
}
```

### Delete an App
```DELETE /apps/:id```

```
$ curl https://api.serverpilot.io/v1/apps/B1w7yc1tfUPQLIKS \
   -u {CLIENTID}:{APIKEY} \
   -X DELETE
```

```json
{
  "actionid": "88Ypexhx28Y63eyA",
  "data": {}
}
```


### Update an App
```POST /apps/:id```

| Name      | Type           | Description
| --------- | :------------: | :---------------------------------------
| `runtime` | `string`       | The PHP runtime for an App. Choose from `php5.4` or `php5.5`.
| `domains` | `array`        | An array of domains that will be used in the webserver's configuration. If you set your app's domain name to *example.com*, Nginx and Apache will be configured to listen for both *example.com* and *www.example.com*. **Note**: The complete list of domains must be included in every update to this field.

```
$ curl https://api.serverpilot.io/v1/apps/nlcN0TwdZAyNEgdp \
   -u {CLIENTID}:{APIKEY} \
   -d '{"runtime": "php5.5"}'
```

```json
{
  "actionid": "KlsNzLikw3BRvShc",
  "data":
  {
    "id": "nlcN0TwdZAyNEgdp",
    "name": "gallery",
    "sysuserid": "RvnwAIfuENyjUVnl",
    "domains": ["www.myartgallery.com", "myartgallery.com"],
    "ssl": null,
    "serverid": "4zGDDO2xg30yEeum",
    "runtime": "php5.5",
  }
}
```

### Add a SSL Cert
```POST /apps/:id/ssl```

| Name      | Type     | Description
| --------- | :------: | :---------------------------------------
| `key`     | `string` | **Required**. The contents of the private key.
| `cert`    | `string` | **Required**. The contents of the certificate.
| `cacerts` | `string` | **Required**. The contents of the CA certificate(s). If none, `null` is acceptable.

```
$ curl https://api.serverpilot.io/v1/apps/nlcN0TwdZAyNEgdp/ssl \
   -u {CLIENTID}:{APIKEY} \
   -H "Content-Type: application/json" \
   -d '{ \
         "key": "-----BEGIN PRIVATE KEY----- ...", \
         "cert": "-----BEGIN CERTIFICATE----- ...", \
         "cacerts": "-----BEGIN CERTIFICATE----- ..." \
       }'
```

```json
{
  "actionid": "BzcMNZ9sdBY62vTd",
  "data":
  {
    "key": "-----BEGIN PRIVATE KEY----- ...",
    "cert": "-----BEGIN CERTIFICATE----- ...",
    "cacerts": "-----BEGIN CERTIFICATE----- ...",
  }
}
```

### Delete SSL for an App
```DELETE /apps/:id/ssl```

```
$ curl https://api.serverpilot.io/v1/apps/nlcN0TwdZAyNEgdp/ssl \
   -u {CLIENTID}:{APIKEY} \
   -X DELETE
```

```json
{
  "actionid": "I1nUHIUR0yzBLESI",
  "data": {}
}
```

## Databases

Databases are MySQL databases.

There is only one Database User for each Database.

### List all Databases
```GET /dbs```

```
$ curl https://api.serverpilot.io/v1/dbs \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data": [
  {
    "id": "hdXkAZchuj27Hm1L",
    "name": "wordpress",
    "appid": "c77JD4gZooGjrF8K",
    "serverid": "4zGDDO2xg30yEeum",
    "user": {
      "id": "vt08Qz9kjOC3RVLr",
      "name": "robert"
    }
  }]
}
```

### Create a Database
```POST /dbs```

| Name             | Type     | Description
| ---------------- | :------: | :---------------------------------------
| `appid`          | `string` | **Required**. The id of the App.
| `name`           | `string` | **Required**. The name of the database.
| `user`           | `object` | **Required**. A JSON object containing `name` and `password` name/value pairs.
| `user[name]`     | `string` | **Required**. The name of the Database User.
| `user[password]` | `string` | **Required**. The password of the Database User.

```
$ curl https://api.serverpilot.io/v1/dbs \
   -u {CLIENTID}:{APIKEY} \
   -H "Content-Type: application/json" \
   -d '{ \
          "appid": "nlcN0TwdZAyNEgdp", \
          "name": "gallerydb", \
          "user": { \
              "name": "arturo", \
              "password": "8apNPT" \
          }
       }'
```

```json
{
  "actionid": "gPFiWP9hFNUxvT70",
  "data":
  {
    "id": "8PV1OIAlAW3jbGmM",
    "name": "gallerydb",
    "appid": "nlcN0TwdZAyNEgdp",
    "serverid": "4zGDDO2xg30yEeum",
    "user": {
      "id": "k2HWtU33mpUsfOdA",
      "name": "arturo"
    }
  }
}
```

### Retrieve an existing Database
```GET /dbs/:id```

```
$ curl https://api.serverpilot.io/v1/dbs/8PV1OIAlAW3jbGmM \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data":
  {
    "id": "8PV1OIAlAW3jbGmM",
    "name": "gallerydb",
    "appid": "nlcN0TwdZAyNEgdp",
    "serverid": "4zGDDO2xg30yEeum",
    "user": {
      "id": "k2HWtU33mpUsfOdA",
      "name": "arturo"
    }
  }
}
```

### Delete a Database
```DELETE /dbs/:id```

```
$ curl https://api.serverpilot.io/v1/dbs/8PV1OIAlAW3jbGmM \
   -u {CLIENTID}:{APIKEY} \
   -X DELETE
```

```json
{
  "actionid": "AtzNxxz4fZv3zJ9D",
  "data": {}
}
```

### Update the Database User Password
```POST /dbs/:id```

| Name             | Type     | Description
| ---------------- | :------: | :----------
| `user`           | `object` | **Required**. A JSON object containing `id` and `password` name/value pairs.
| `user[id]`       | `string` | **Required**. The id of the Database User.
| `user[password]` | `string` | **Required**. The *new* password of the Database User.

```
$ curl https://api.serverpilot.io/v1/dbs/8PV1OIAlAW3jbGmM \
   -u {CLIENTID}:{APIKEY} \
   -d '{"user": {"id": "k2HWtU33mpUsfOdA", "password": "8aTWa7"}}'
```

```json
{
  "actionid": "VfH12ukDJFv0RZAO",
  "data":
  {
    "id": "8PV1OIAlAW3jbGmM",
    "name": "gallerydb",
    "appid": "nlcN0TwdZAyNEgdp",
    "serverid": "4zGDDO2xg30yEeum",
    "user": {
      "id": "k2HWtU33mpUsfOdA",
      "name": "arturo"
    }
  }
}
```

## Actions

Actions are a record of work done on ServerPilot resources. These can be things
like the creation of an App, deploying SSL, deleting an old Database, etc.

All methods that modify a resource will have an `actionid` top-level key in the
JSON response. The `actionid` can be used to track the `status` of the Action.

**Possible values of Action `status`**

| Status    | Description
| --------- | -----------
| `success` | Action was completed successfully.
| `open`    | Action has not completed yet.
| `error`   | Action has completed but there were errors.

### Check status of Action
```GET /actions/:id```

```
$ curl https://api.serverpilot.io/v1/actions/g3kiiYzxPgAjbwcY \
   -u {CLIENTID}:{APIKEY}
```

```json
{
  "data":
  {
    "id": "g3kiiYzxPgAjbwcY",
    "serverid": "4zGDDO2xg30yEeum",
    "status": "success",
    "datecreated": 1403138066,
  }
}
```
