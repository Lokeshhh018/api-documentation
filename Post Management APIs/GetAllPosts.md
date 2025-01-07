# Get All Posts API

## **Endpoint**
`GET /post`

## **Description**
```
Retrieves the latest 20 blog posts.
```

## **Code**
```
app.get('/post', async (req, res) => {
  const posts = await Post.find()
    .populate('author', ['username'])
    .sort({ createdAt: -1 })
    .limit(20);
  res.json(posts);
});
```

## **Responses**
```
200 OK Returns an array of posts.

Example:

 { "_id": "string",
   "title": "string",
   "summary": "string",
   "content": "string",
   "cover": "string",
   "author": { "_id": "string", "username": "string" },
   "createdAt": "string",
   "updatedAt": "string" } 
```