# Get Profile API

## **Endpoint**
`GET /profile`

## **Description**
```
Retrieves the profile information of the logged-in user.
```

## **Code**
```
app.get('/profile', (req, res) => {
  const { token } = req.cookies;
  jwt.verify(token, secret, {}, (err, info) => {
    if (err) {
      return res.status(401).json({ message: 'Unauthorized' });
    }
    res.json(info);
  });
});
```



## **Request**
The request should include the following cookie:
```
{ "token": "string" }
```


## **Responses**
```
200 OK Returns the user information.

Example:

{ "username": "string", "id": "string" }

401 Unauthorized Occurs when the token is invalid or missing.