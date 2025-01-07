# Logout User API

## **Endpoint**
`POST /logout`

## **Description**
```
Logs out the user by clearing the token cookie.
```

## **Code**
```
app.post('/logout', (req, res) => {
  res.cookie('token', '').json('ok');
});
```



## **Responses**
```
200 OK Returns a confirmation message.

Example:

"ok"