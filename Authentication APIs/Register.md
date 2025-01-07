# Register User API

## **Endpoint**
`POST /register`

## **Description**
```
Registers a new user with a username and password.
```
## **Code**
```
app.post('/register', async (req, res) => {
  const { username, password } = req.body;
  if (!username || !password) {
    return res.status(400).json({ message: 'Username and password are required' });
  }
  
  try {
    const userDoc = await User.create({
      username,
      password: bcrypt.hashSync(password, salt),
    });
    res.json(userDoc);
  } catch (e) {
    console.log(e);
    res.status(400).json(e);
  }
});
```

## **Request Body**
The request should include the following JSON object:

```
{
  "username": "string",
  "password": "string"
}

```
## **Responses**

```
200 OK
Returns the created user document.

Example:

{
  "_id": "string",
  "username": "string",
  "password": "string"
}

400 Bad Request
Occurs when required fields are missing or there are other errors.

Example:

{
  "message": "Username and password are required"
}
