# Login User API

## **Endpoint**
`POST /login`

## **Description**
```
Logs in a user and returns a JWT token in a cookie.
```

## **Code**
```
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  if (!username || !password) {
    return res.status(400).json({ message: 'Username and password are required' });
  }

  try {
    const userDoc = await User.findOne({ username });
    if (!userDoc) {
      return res.status(400).json({ message: 'User  not found' });
    }

    const passOk = bcrypt.compareSync(password, userDoc.password);
    if (passOk) {
      // logged in
      jwt.sign({ username, id: userDoc._id }, secret, {}, (err, token) => {
        if (err) throw err;
        res.cookie('token', token).json({
          id: userDoc._id,
          username,
        });
      });
    } else {
      res.status(400).json({ message: 'Wrong credentials' });
    }
  } catch (error) {
    console.error(error);
    res.status(500).json({ message: 'Internal Server Error' });
  }
});
```



## **Request Body**
The request should include the following JSON object:
```

{ "username": "string", "password": "string" }

```

## **Responses**
```

200 OK Returns the user ID and username, and sets the JWT token in a cookie.

Example:

{ "id": "string", "username": "string" }

400 Bad Request Occurs when credentials are incorrect or required fields are missing.

500 Internal Server Error Occurs due to a server-side error.