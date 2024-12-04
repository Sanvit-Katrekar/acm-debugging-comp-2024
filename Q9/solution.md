# Solutions

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
Now, because if this error, an empty schema is already created.
* Delete that empty db file
* Because of opening in 'w' mode file, `schema.sql` becomes empty, need to  put the contents back in.

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
