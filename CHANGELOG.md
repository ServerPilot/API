# Changelog

## 8-22-2014

  - Reformatted curl commands to appear to use environment variables. Replace
    '{CLIENTID}:{APIKEY}' with '$CLIENTID:$APIKEY'.
  - Document possible HTTP status codes.
  - Document structure of JSON object in body of 4xx and 5xx responses.
  - Document validation policies for `name` and `password` fields.

## 7-24-2014
    
  - Added some clarification to the "Update an App" section. Add and remove
    operations for the `domains` array are not supported. You must provide the
    *complete* list of domains on any update to the `domains` field.

