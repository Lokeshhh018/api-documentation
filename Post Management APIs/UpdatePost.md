# Update Post API

## **Endpoint**
`PUT /post`

## **Description**
```
Updates an existing blog post.

```

## **Code**
```
app.put('/post', uploadMiddleware.single('file'), async (req, res) => {
  let newPath = null;
  if (req.file) {
    const { originalname, path } = req.file;
    const parts = originalname.split('.');
    const ext = parts[parts.length - 1];
    newPath = path + '.' + ext;

    try {
      fs.renameSync(path, newPath);
    } catch (error) {
      console.error('Error renaming file:', error);
      return res.status(500).json({ message: 'File upload error' });
    }
  }

  const { token } = req.cookies;
  jwt.verify(token, secret, {}, async (err, info) => {
    if (err) {
      return res.status(401).json({ message: 'Unauthorized' });
    }

    const { id, title, summary, content } = req.body;
    const postDoc = await Post.findById(id);
    const isAuthor = JSON.stringify(postDoc.author) === JSON.stringify(info.id);
    if (!isAuthor) {
      return res.status(403).json({ message: 'You are not the author' });
    }

    await postDoc.update({
      title,
      summary,
      content,
      cover: newPath ? newPath : postDoc.cover,
    });

    res.json(postDoc);
  });
});
```

## **Request**

### **Headers**
```
Content-Type: multipart/form-data
```

### **Cookies**
```json
{
  "token": "string"
}
```
### **Body Parameters**
```
{
  "id": "string",
  "title": "string",
  "summary": "string",
  "content": "string",
  "file": "file (optional)"
}
```

## **Responses**
```
200 OK
Returns the updated post document.

400 Bad Request
Occurs when required fields are missing.

401 Unauthorized
Occurs when the token is invalid or missing.

403 Forbidden
Occurs when the user is not the author of the post.

500 Internal Server Error
Occurs when there is a server-side error.
```