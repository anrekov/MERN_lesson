# MERN_lesson

## Backend:

1. Validation

```
const { check, validationResult } = require('express-validator/check');

router.post(
  '/',
  [
    auth,
    [
      check('status', 'Status is required').not().isEmpty(),
      check('skills', 'Skills is required').not().isEmpty(),
    ],
  ],
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }

    // ...
  }
```

2. ref's for another collections

```
// In model:
const ProfileSchema = new mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'user',
  },

// In api route:
const profile = await Profile.findOne({
      user: req.params.user_id,
    }).populate('user', ['name', 'avatar']);
```

3. Github repos

```
router.get('/github/:username', (req, res) => {
  try {
    const options = {
      uri: `https://api.github.com/users/${
        req.params.username
      }/repos?per_page=5&sort=created:asc&client_id=${config.get(
        'githubClientId'
      )}&client_secret=${config.get('githubSecret')}`,
      method: 'GET',
      headers: { 'user-agent': 'node.js' },
    };

    request(options, (error, response, body) => { // ...

    // ...

  }
```

4. Gravatar

```
const gravatar = require('gravatar');

// Get users gravatar
const avatar = gravatar.url(email, {
  s: '200',
  r: 'pg',
  d: 'mm',
});
```

#

## Frontend:

### Axios:

```
// Change headers
const setAuthToken = (token) => {
  if (token) {
    axios.defaults.headers.common['x-auth-token'] = token;
  } else {
    delete axios.defaults.headers.common['x-auth-token'];
  }
};

// Using
const token = req.header('x-auth-token');
```

### Redux:

---

**Redux flow:** "If you want to add anything to your application: any other resourses and functionality you can just simply create a new reducer, new actions file and then create a components. That's kind of the flow of redux."

---

_All_ operations with user in one reducer: create, login, auth, delete:

```
case ACCOUNT_DELETED:
case LOGOUT:
case LOGIN_FAIL:
case AUTH_ERROR:
case REGISTER_FAIL:
  localStorage.removeItem('token');
  return {
    ...state,
    token: null,
    isAuthenticated: false,
    loading: false,
  };
```

## Deployment:

### HEROKU

1. Terminal:
```
heroku login
```

2. Create production.json (might be other parametrs) in config folder and Add ``` config/default.json ``` in .gitignore

3. Add script in package.json:
```
"heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix client && npm run build --prefix client"
```

4. Change server.js:
```
// Serve static assets in production (only after routes)
if (process.env.NODE_ENV === 'production') {
  // Set static folder
  app.use(express.static('client/build'));

  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'));
  });
}
```

5. Terminal: ``` heroku create ```

This app based on git

6. Change git:
```
$ git init

$ heroku git:remote -a secret-reef-08244

$ git add .
$ git commit -am "make it better"
$ git push heroku master
```
7. Check application: 
```
heroku open
```
8. Change domain on heroku.com in settings
_My default domain: https://secret-reef-08244.herokuapp.com _

End
