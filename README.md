# Django - Postgres Template

1. Create a directory, and virtual environment
- Create a project directory, in this case I use “django-postgres” 
```bash
mkdir django-postgres
```
- Point to django-postgres directory
```bash
cd django-postgres
```
- Build virtual environment for the directory
```
python -m venv env
```
- The last step is activating virtual environment, so you’ve to run the command below
```
source env/bin/activate
```
- Check Python version
```bash
python --version
```

2. Install Django
After successfully creating the directory and virtual environment, it’s time to install Django.
Use the package manager [pip](https://pip.pypa.io/en/stable/) to install Django.
```bash
pip install Django
```

3. Start the Django Project

```python
django-admin startproject django_psql
```
I successfully created project called “django_psql”

4. Start Django App
- In VisualStudio Code terminal, you’ve to point to the project directory using “cd” command
```bash
cd django_psql
```
- Run the following command to start your first app.
```python
python manage.py startapp testdb
```

After complete building the app, then you can write your desired table that you should previously design e.g. which types of data you want to store in your database.

- Run the server to make sure that everything is okay
```python
python manage.py runserver
```
python manage.py is the command that’s used to run Django, so you will use it many times a long with your project

5. Setting up a database server
I assume you already download PostgreSQL, if don’t, the following is the link to download →
[PostgreSQL Downloads](https://www.postgresql.org/download/)

6. Get back to our code to config the database
- Navigate to ```settings.py```
- Approximately, in line 76 of code, this is the database config part

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
By default, the sqlite database is used. we will change this config for psql.
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'your_db_name', 
        'USER': 'postgres',
        'PASSWORD': 'your_db_password',
        'HOST': '127.0.0.1', 
        'PORT': '5432',
    }
}
```
- Remember: Don’t keep sensitive data in the setting file, instead, use the environment variable AKA .env file.

7. Create a Table
Get back to app in testdb/models.py

- Create a table by writing a class/object(ORM) to access the database instead of writing raw SQL (Django automatically converts Python class/object code to be raw SQL under the hood.

This is just a simple table for testing

```python
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=80)
    age = models.IntegerField()
```

8. Migrate the table to the PostgreSQL database
Now, we have successfully written our table but it isn’t being sent to PostgreSQL yet. So what I would do are the following steps
- makemigrations → To update and see the history or transaction happened in our table (We have to run this command every time when something changes in models.py e.g. adding a new table, changing a field name, etc
- migrate → The last step to submit or sent out our table into the database server

Now it’s time to run “makemigrations” command
```python
python manage.py makemigrations
```
I got this error “No changes detected” because I didn’t register my app.

- Go to settings.py to register the app
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'testdb',
    ...
]
```

- No problem with the app, but I got new error “No module named psycog2”

For macOS if you can’t install or the above command doesn’t work, use the following command instead, just add — binary

```bash
pip install psycopg2
```

Now everything is okay, so I can run “makemigrations” again

```python
python manage.py makemigrations
```

The last step is running “migrate”

```python
python manage.py migrate
```


## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)