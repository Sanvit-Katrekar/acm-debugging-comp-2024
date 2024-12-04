The Profiles App development journey has been incredibly rewarding! The process has taught me so much about building dynamic web applications and working with user data securely.

I created a profiles app, where users can sign up, create and edit their profiles, and interact with others in a personalized way.

I challenged myself by using the latest technologies and frameworks to ensure the app is modern and scalable. 
From integrating authentication to implementing smooth UI/UX elements, it’s been a thrilling experience bringing everything together.

I can’t wait to continue expanding the app and exploring even more features!

I need you to please test my site and help by fixing any errors you find, so I can move on to the next feature!

Note 1: Do not remove any libraries import statements
Note 2: The schema is already defined in the `schema.sql` file for initializing the db.
Note 3: Ignore any css issues if any, focus on the main application

## Issues / Solutions

1. Issue with werkzeug utils `url_quote_plus` function

In env > lib > python3.10 > site-packages > flask-restless > views.py,
```py
#from werkzeug.urls import url_quote_plus
from urllib.parse import quote as url_quote_plus
```

2. File `schema.sql` should actually opened with read mode

3. Open schema.sql in `read` mode:
```py
def init_db():
    with app.app_context():
        db = get_db()
        with open('schema.sql', mode='r') as f:
            db.cursor().executescript(f.read())
```
4. `id` is missing from fetch call

Change:

```py
@app.route('/submissions')
def submissions():
    if 'user_id' not in session:
        flash('Please log in to view submissions.', 'warning')
        return redirect(url_for('login'))
    db = get_db()
    cur = db.execute('SELECT name, email, message FROM submissions')
    submissions = cur.fetchall()
    return render_template('submissions.html', submissions=submissions)
```

To:

```py
@app.route('/submissions')
def submissions():
    if 'user_id' not in session:
        flash('Please log in to view submissions.', 'warning')
        return redirect(url_for('login'))
    db = get_db()
    cur = db.execute('SELECT name, email, message, id FROM submissions')
    submissions = cur.fetchall()
    return render_template('submissions.html', submissions=submissions)
```
5. `id` not passed as a function argument for /delete_submission endpoint

6. In `/profile` endpoint, change `fetchall()` to `fetchone()`
