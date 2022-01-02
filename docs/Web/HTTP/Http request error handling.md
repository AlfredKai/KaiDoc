# Http Request Error Handling

1. not all apis use restful design.
2. although some apis use customizing error code rather than http status code, they still may use some http status code to return error (404 for example).
3. you'll get exceptions for network error and get normal response with not ok http status code.
