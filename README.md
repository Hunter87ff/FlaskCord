# FlaskCord
[![Latest Release](https://img.shields.io/github/v/release/hunter87ff/FlaskCord?include_prereleases&label=Latest%20Release&logo=github&sort=semver&style=for-the-badge&logoColor=white)](https://github.com/hunter87ff/FlaskCord/releases)

A flask-discord fork. flaskcord is a feature rich extension for Flask. with discord.py like functionalities


### Installation
To install current development version you can use following command:
```sh
pip install git+https://github.com/hunter87ff/FlaskCord.git
```


### Basic Example
```python
import os

from flask import Flask, redirect, url_for
from flaskcord import Session, requires_authorization

app = Flask(__name__)
app.secret_key = b"%\xe0'\x01\xdeH\x8e\x85m|\xb3\xffCN\xc9g"
os.environ["OAUTHLIB_INSECURE_TRANSPORT"] = "true"    # !! Only in development environment.
app.config["DISCORD_CLIENT_ID"] = 490732332240863233
app.config["DISCORD_CLIENT_SECRET"] = os.getenv("DISCORD_CLIENT_SECRET")
app.config["DISCORD_BOT_TOKEN"] = os.getenv("DISCORD_BOT_TOKEN")
app.config["DISCORD_REDIRECT_URI"] = "http://127.0.0.1:5000/callback"
discord = Session(app)


@app.route("/login/")
def login():
    return discord.create_session()
	

@app.route("/callback/")
def callback():
    discord.callback()
    return redirect(url_for(".me"))


@app.errorhandler(Unauthorized)
def redirect_unauthorized(e):
    return redirect(url_for("login"))

	
@app.route("/me/")
@requires_authorization
def me():
    user = discord.fetch_user()
    return f"""
    <html>
        <head>
            <title>{user.name}</title>
        </head>
        <body>
            <img src='{user.avatar_url}' />
        </body>
    </html>"""


if __name__ == "__main__":
    app.run()
```

For an example to the working application, check [`test_app.py`](example/test_app.py)


### Requirements
* Flask
* requests_oauthlib
* cachetools
* discord.py


<!-- ### Documentation
Head over to [documentation](https://FlaskCord.readthedocs.io/en/latest/) for full API reference.  -->


