# Flask Login Basics


**Login Manager**
`flask_login.LoginManager`

```python
from flask_login import LoginManager

login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = "login"
```

**User model**
User should be of class `UserMixin`. This class provides default implementations for the methods that Flask-Login expects user objects to have which are `is_authenticated`, `is_active`, `is_anonymous`, and `get_id`.
```python
class User(UserMixin):
    def __init__(self, id):
        self.id = id
```

**User Loader**
`login_manager.user_loader` is a decorator that registers a function to load a user. This function should return a user object or `None`.
```python
@login_manager.user_loader
def load_user(user_id):
    return User(user_id)
```

**Login of a user**
- `flask_login.login_user`
After a user logs in, you can call `login_user` to log them in. This function takes a user object as an argument.

```python
    # don't check password like this in production
    if email in users and users[email]["password"] == password:
        user = User(email)
        login_user(user)
```

**Current User**
Flask-Login provides a current_user proxy that gives you access to the logged-in user object from anywhere in your application. This is particularly useful in templates and route functions.
from flask_login import current_user
```python
@app.route("/")
def home():
    if current_user.is_authenticated:
        return f"Hello {current_user.id}"
    return "Hello Guest"
```

**Required Login**
The `@login_required` decorator works by checking `current_user.is_authenticated` before running the route. If authentication fails, it redirects to the login page you specified with `login_manager.login_view = "login"`.

```python
@app.route('/protected')
@login_required
def protected():
    return f'Hello, {current_user.id}! This is a protected page.'
```


**Logout**
Flask-Login provides a `logout_user()` function that removes the user's login state:

```python
from flask_login import logout_user

@app.route("/logout")
@login_required
def logout():
    logout_user()
    return redirect(url_for('home'))
```

**Session Protection**
Flask-Login requires a secret key to be set for session security. Best practice is to set a secret key in your Flask app using environmental variables. This key is used to cryptographically sign the session cookie.

```python
app = Flask(__name__)
app.secret_key = os.environ.get("SECRET_KEY")  # Recommended to use environment variable for secure session management
app.config['SESSION_COOKIE_SECURE'] = True  # Only send cookie over HTTPS
app.config['SESSION_COOKIE_HTTPONLY'] = True  # Prevent JavaScript access to session cookie
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(minutes=30)  # Session expires after 30 minutes
app.config['SESSION_COOKIE_SAMESITE'] = 'Lax'  # Protect against CSRF

login_manager = LoginManager()
login_manager.init_app(app)
login_manager.session_protection = "strong"  # Can be "basic", "strong", or None
```


**Common patterns**
Following shows a simple login route that redirects the user to the page they wanted before being redirected to login.
This is useful when a user tries to access a protected page without being logged in.

```python
@app.route("/login", methods=['GET', 'POST'])
def login():
    # If user is already logged in, redirect them
    if current_user.is_authenticated:
        return redirect(url_for('home'))

    if request.method == 'POST':
        email = request.form['email']
        password = request.form['password']
        if validate_login(email, password):  # Your validation logic
            user = User(email)
            login_user(user)
            # Get the page they wanted before being redirected to login
            next_page = request.args.get('next')
            return redirect(next_page or url_for('home'))
        flash('Invalid credentials')
    return render_template('login.html')
```
