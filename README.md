Flask App
==========

Flask App using the Flask microframework.


Requirements
============

Python 2.7+, because that's just what Flask recommends.  

Everything else is listed in the `requirements.txt` file.  
You might also want to make sure that you have `sqlite3` installed on your system so that you can generate the database.


Getting started
===============


Setup is fairly easy for this application due to the nature of how it's built. However, I will inform you ahead of time that this is not designed for high-traffic load. Due to things like SQLite usage.

1. Clone the repository into a folder anywhere you wish
2. (Optional) Create a virtual environment for the project
3. Install everything listed in requirements.txt using `pip install -r requirements.txt` or do it manually
4. Proceed with the relevant procedure fitting your use case below

Development
-----------

1. Generate the sqlite database by running `sqlite3 blog.db < schema.sql` in the directory where you cloned the repo
2. Do `python exec.py`
3. Start working

By default, the application will listen on all IPs/Domains and run on port 5800.

Production
----------

1. Edit the variables in `exec.py` to fit your needs, **Make sure you remove `debug=True` from `app.run()` for security reasons, you should also make sure that `host="<something>"` is set to `127.0.0.1` for step 3**
2. Generate the sqlite database by running `sqlite3 blog.db < schema.sql` in the directory where you cloned the repo
3. Set up a proper webserver to handle your requests. I recommend nginx. Configure it as a reverse proxy for your desired subdomain or url, pointing to `127.0.0.1` and whatever port you defined in `exec.py`
4. Do `python exec.py`
5. (Optional) It's recommended that you configure your webserver to handle static files for you. This can greatly improve performance and in some cases, security. If you don't know how to do it, the application will server static files for you worst-case.

Migration
---------

Now this is the best part

1. Do exactly the same as above, but instead of creating the database with the `schema.sql` file, just bring along your own `blog.db` file

Backup
------

1. Copy the `blog.db` somewhere

why can't everything be this easy

Updating
--------

1. Backup your `blog.db` file.
2. Do a `git pull` to get the most recent code
3. Edit any configs in the file so that it works with your existing setup
4. Sometimes, changes are made to the database structure. To update your database, use the provided sql files. eg. to update from v1.2 to v1.3, which added support for timestamps on posts, run `sqlite3 blog.db < upgrade-v12-v13.sql`
4. Start the application normally using `python exec.py

Usage
=====


Posts are written using Markdown. 

* You can log in using `example.com/login`, you must do this before anything else on this list will work
* A logout link and a link to the post editor will appear at the bottom
* To edit a post, use the link added to the post page. Alternatively, add `/edit` to the post url. To edit `example.com/post/2`, you'd visit `example.com/post/2/edit`
* To delete a post, replace `post` in the URL with `del`. To delete `example.com/post/3`, you'd just visit `exaple.com/del/3` **WARNING: No confimation, your post will be deleted the instant the server recieves the GET request from your browser**

-----------
Rest API
---------------
a.	Allows users to submit/post messages

User to post new message:
Usage:
curl -d "title=<title> &text=”<message>" -X POST <url>/add

Ex:
$ curl -d "title=If you want it, work for it.&text=If you want it, work for it." -X POST http://127.0.0.1:5000/add
{
  "status": "Added Post: If you want it, work for it.:If you want it, work for it."
}


$ curl -d "title=Whatever you are, be a good one&text=Whatever you are, be a good one" -X POST http://127.0.0.1:5000/add
{
  "status": "Added Post: Whatever you are, be a good one:Whatever you are, be a good one"
}


$ curl -d "title=Do it with passion or not at all.&text=Do it with passion or not at all." -X POST http://127.0.0.1:5000/add
{
  "status": "Added Post: Do it with passion or not at all.:Do it with passion or not at all."
}



$ curl -d "title=Take the risk or lose the chance&text=Take the risk or lose the chance" -X POST http://127.0.0.1:5000/add
{
  "status": "Added Post: Take the risk or lose the chance:Take the risk or lose the chance"
}



$ curl -d "title=Don’t dream your life, live your dream.&text=Don’t dream your life, live your dream." -X POST http://127.0.0.1:5000/add
{
  "status": "Added Post: Don\u2019t dream your life, live your dream.:Don\u2019t dream your life, live your dream."
}


b.	Lists received messages:
Usage:
curl -X GET <url>/all

Ex:
$ curl -X GET http://127.0.0.1:5000/all
{
  "Messages": [
    {
      "id": 23,
      "time": "August 20, 2018 (07:57 - India Standard Time)",
      "title": "Don\u2019t dream your life, live your dream."
    },
    {
      "id": 22,
      "time": "August 20, 2018 (07:57 - India Standard Time)",
      "title": "Take the risk or lose the chance"
    },
    {
      "id": 21,
      "time": "August 20, 2018 (07:56 - India Standard Time)",
      "title": "Do it with passion or not at all."
    },
    {
      "id": 20,
      "time": "August 20, 2018 (07:56 - India Standard Time)",
      "title": "Whatever you are, be a good one"
    },
    {
      "id": 19,
      "time": "August 20, 2018 (07:56 - India Standard Time)",
      "title": "If you want it, work for it."
    },
    {
      "id": 18,
      "time": "August 20, 2018 (07:53 - India Standard Time)",
      "title": "Hello Worlds"
    },
    {
      "id": 16,
      "time": null,
      "title": "PalindromeCheck"
    }
  ]
}

c.	Retrieves a specific message on demand, and determines if it is a palindrome.
To check is message is palindrome or not: 
Message not Palindrome:
Usage:
curl -X GET <url>/post/<postid>

Ex:

$ curl -X GET http://127.0.0.1:5000/post/21
{
  "Palindrome Results": "Message :Do it with passion or not at all.: not palindrome",
  "respose": {
    "text": "<p>Do it with passion or not at all.</p>",
    "time": 1534732009.9673285,
    "title": "Do it with passion or not at all.",
    "updated": null
  }
}

Message Is Palindrome:
Usage:
curl -X GET <url>/post/<postid>

$ curl -X GET http://127.0.0.1:5000/post/24
{
  "Palindrome Results": "Message :Malayalam: is a palindrome",
  "respose": {
    "text": "<p>Malayalam</p>",
    "time": 1534732953.5809932,
    "title": "PalindromeCheck",
    "updated": null
  }
}



d.	Allows users to delete specific messages

Usage:
curl -X POST <url>/del/<postid>

{

Ex:
$ curl -X POST http://127.0.0.1:5000/del/18
{
  "status": "Deleted Post: 18"
}




e.	To update a message:
Usage:
curl -d "title=<Update content>&text=<sample message>" -X POST <URL>/update/<postid>

Ex:
$ curl -d "title=Update content&text=sample message" -X POST http://127.0.0.1:5000/update/16

{
  "status": "Updated: Update content:sample message"
}


