# Get Single Post API

## **Endpoint**
`GET /post/:id`

## **Description**
```
Retrieves a single blog post by its ID.
```
## **Code**
```
app.get('/post/:id', async (req, res) => {
  const { id } = req.params;
  const postDoc = await Post.findById(id).populate('author', ['username']);
  if (!postDoc) {
    return res.status(404).json({ message: 'Post not found' });
  }
  res.json(postDoc);
});
```

## **Request Parameters**
```
id: ID of the post.
```





## **Responses**
```
200 OK Returns the post document.

Example:

{ "_id": "string",
  "title": "string",
  "summary": "string",
  "content": "string", 
  "cover": "string", 
  "author": { "_id": "string", "username": "string" }, 
  "createdAt": "string", 
  "updatedAt": "string" }

Error Codes

400 Bad Request The request was invalid or missing required parameters.

401 Unauthorized Authentication is required or the token is invalid.

403 Forbidden User does not have permission to access the resource.

404 Not Found The requested resource could not be found.

500 Internal Server Error An unexpected error occurred on the server.
```