# Delete Single Post API

## **Endpoint**
`DELETE /post/:id`

## **Description**
```
Deletes a single blog post by its ID.
```
##**Code**
```
app.get('/delete/:id', (req, res) => {
    const postId = parseInt(req.params.id);
    const postIndex = posts.findIndex(post => post.id === postId);

    if (postIndex !== -1) {
        posts.splice(postIndex, 1);
        res.status(200).send({ message: 'Post deleted successfully' });
    } else {
        res.status(404).send({ message: 'Post not found' });
    }
});
```

## **Request Parameters**
```

id: ID of the post to be deleted.
```


## **Responses**
```
200 OK Returns a confirmation message.
Example:

{ "message": "Post deleted successfully." }

Error Codes

400 Bad Request The request was invalid or missing required parameters.

401 Unauthorized Authentication is required or the token is invalid.

403 Forbidden User does not have permission to delete the post.

404 Not Found The requested resource could not be found.

500 Internal Server Error An unexpected error occurred on the server.
```