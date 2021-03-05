# MERN_lesson

Backend:

1. Validation
2. axios
3. ref's for another collections

Redux:

```
const token = req.header('x-auth-token');

// Change headers
const setAuthToken = (token) => {
  if (token) {
    axios.defaults.headers.common['x-auth-token'] = token;
  } else {
    delete axios.defaults.headers.common['x-auth-token'];
  }
};
```

All operations with user in one reducer: create, login, auth, delete
