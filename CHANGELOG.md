# Changelog

## 2017-10-09

  - Added PHP 7.2 runtime.

## 2017-07-19

  - Added server `deny_unknown_domains` field.

## 2016-08-30

  - Added `datecreated` field to app information in responses.

## 2016-08-23

  - Passwords cannot be longer than 200 characters.

## 2016-08-11

  - Added AutoSSL and ForceSSL management.

## 2016-07-29

  - Added PHP 7.1 runtime.

## 2015-08-21

  - Added PHP 7.0 runtime.
  - Clarified `actionid` only available in response if server configuration required.

## 2014-09-03

  - Added references to PHP 5.6

## 2014-08-26

  - Tried to define more clearly what the steps are to connecting a new server
    to ServerPilot.

## 2014-08-22

  - Reformatted curl commands to appear to use environment variables. Replace
    '{CLIENTID}:{APIKEY}' with '$CLIENTID:$APIKEY'.
  - Document possible HTTP status codes.
  - Document structure of JSON object in body of 4xx and 5xx responses.
  - Document validation policies for `name` and `password` fields.

## 2014-07-24
    
  - Added some clarification to the "Update an App" section. Add and remove
    operations for the `domains` array are not supported. You must provide the
    *complete* list of domains on any update to the `domains` field.

