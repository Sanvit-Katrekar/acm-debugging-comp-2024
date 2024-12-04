# My Portfolio Website!

The Dev Bootcamp series was amazing! Learnt a lot of new stuff!

I thought that it would be better for me if I tried to code out the same thing by myself using all the latest versions of the software involved.

But.. a bunch of issues came up, someone, please help me out..

## Issues

1. API Key variable not found.. what could be the reason.. maybe I missed something?

2. Unable to load templates and static files.. I did exactly the process followed in the bootcamp, still nope..

3. A random SQL error pops up.. no idea how to fix it..

4. Unable to submit forms: I was playing around with the forms, now getting some error..

5. The feedback carousel does not work anymore..


### Solution:

1. Change variable name `API_SECRET` to `API_KEY`
```py
API_KEY = os.getenv('API_KEY')
```
2. Wrong directories specified, change:

```py
app = Flask(
    __name__,
    static_folder='app/static',
    template_folder='app/templates'
)
```

to:

```py
app = Flask(
    __name__,
    static_folder='static',
    template_folder='template'
)

# (or)

app = Flask(
    __name__
) # rename 'template' folder to 'templates'
```

3. Random SQL error

SQL error occurs since the table itself is not created.
Need to call `db.create_all()` after the ORM class definitions.

Change:

```py
with app.app_context():
    db.create_all()

class Feedback(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), nullable=False)
    job_title = db.Column(db.String(120))
    feedback_text = db.Column(db.Text, nullable=False)

    def __repr__(self):
        return f"<Feedback {self.username}>"
```

to:

```py
with app.app_context():
    db.create_all()

class Feedback(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), nullable=False)
    job_title = db.Column(db.String(120))
    feedback_text = db.Column(db.Text, nullable=False)

    def __repr__(self):
        return f"<Feedback {self.username}>"
```


4. Form variables sent to server do not match, set correct input names:

```html
<h3>Submit your feedback!</h3>
<form action="/submit_feedback" method="POST" class="comment-form">
    <div class="form-group">
        <input type="text" name="name" class="form-control" placeholder="Your Name" required>
    </div>
    <div class="form-group">
        <input type="text" name="position" class="form-control" placeholder="Your Position">
    </div>
    <div class="form-group">
        <textarea name="comment" class="form-control" placeholder="Your Comment" rows="3" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary btn-submit">Submit Feedback</button>
</form>
```

change to:

```html
<h3>Submit your feedback!</h3>
<form action="/submit_feedback" method="POST" class="comment-form">
    <div class="form-group">
        <input type="text" name="username" class="form-control" placeholder="Your Name" required>
    </div>
    <div class="form-group">
        <input type="text" name="job_title" class="form-control" placeholder="Your Position">
    </div>
    <div class="form-group">
        <textarea name="feedback" class="form-control" placeholder="Your Comment" rows="3" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary btn-submit">Submit Feedback</button>
</form>
```

4. Feedback carousel doesn't work since a few changes are made in the latest Bootstrap version
    * First off, import the Boostrap 5.3.3 JS bundle
    * Need to change code to be 5.3.3 compliant, can get this by reading bootstrap docs

Change:
```html
<!-- Carousel for Comments-->
<div id="commentCarousel" class="carousel slide" data-ride="carousel">
    <div class="carousel-inner">
        {% for feedback in feedbacks %}
            <div class="carousel-item {% if loop.first %}active{% endif %}">
                <div class="comment-box">
                    <strong>{{ feedback['USERNAME'] }}</strong> ({{ feedback['JOB_TITLE'] }}):
                    <p>{{ feedback['FEEDBACK'] }}</p>
                </div>
            </div>
        {% endfor %}
    </div>
    <a class="carousel-control-prev" href="#commentCarousel" role="button" data-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>
    </a>
    <a class="carousel-control-next" href="#commentCarousel" role="button" data-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="sr-only">Next</span>
    </a>
</div>
```
to:
```html
<!-- Comments Section -->
<section id="feedback" class="section comments-section">
    <div class="container">
        <h2 class="text-center">Feedback</h2>
            <div class="quote">"{{daily_quote}}"</div>
        <!-- Carousel for Comments-->
        <div id="commentCarousel" class="carousel slide" data-bs-ride="carousel">
            <div class="carousel-inner">
            {% for feedback in feedbacks %}
                <div class="carousel-item {% if loop.first %}active{% endif %}">
                <div class="comment-box">
                    <strong>{{ feedback['USERNAME'] }}</strong> ({{ feedback['JOB_TITLE'] }}):
                    <p>{{ feedback['FEEDBACK'] }}</p>
                </div>
                </div>
            {% endfor %}
            </div>
            <button class="carousel-control-prev" type="button" data-bs-target="#commentCarousel" data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
            </button>
            <button class="carousel-control-next" type="button" data-bs-target="#commentCarousel" data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
            </button>
        </div>
```
