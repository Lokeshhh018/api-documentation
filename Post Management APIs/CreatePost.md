# Create Post API

## **Endpoint**
`POST /post`

## **Description**
```
Creates a new blog post.
```

## **Code**
```
app.post('/post', uploadMiddleware.single('file'), async (req, res) => {
    // Check if a file was uploaded
    if (!req.file) {
      return res.status(400).json({ message: 'File is required' });
    }
  
    const { originalname, path } = req.file;
    const parts = originalname.split('.');
    const ext = parts[parts.length - 1];
    const newPath = path + '.' + ext;
  
    try {
      fs.renameSync(path, newPath);
    } catch (error) {
      console.error('Error renaming file:', error);
      return res.status(500).json({ message: 'File upload error' });
    }
  
    const { token } = req.cookies;
    jwt.verify(token, secret, {}, async (err, info) => {
      if (err) {
        console.error('JWT verification error:', err);
        return res.status(401).json({ message: 'Unauthorized' });
      }
  
      const { title, summary, content } = req.body;
  
      // Check if required fields are present
      if (!title || !summary || !content) {
        return res.status(400).json({ message: 'Title, summary, and content are required' });
      }
  
      try {
        const postDoc = await Post.create({
          title,
          summary,
          content,
          cover: newPath,
          author: info.id,
        });
        res.json(postDoc);
      } catch (error) {
        console.error('Error creating post:', error);
        res.status(500).json({ message: 'Internal Server Error' });
      }
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
  "title": "string",
  "summary": "string",
  "content": "string",
  "file": "file"
}
```

## **Responses**
```
200 OK
Returns the created post document.

400 Bad Request
Occurs when required fields or file are missing.

401 Unauthorized
Occurs when the token is invalid or missing.

500 Internal Server Error
Occurs when there is a server-side error.
