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
