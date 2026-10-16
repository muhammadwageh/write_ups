## Change the Request Method.

Now, you test the endpoint and it returns 404 or 403. Just try to change the Request Method:

GET, POST, PUT, PATCH, DELETE

You will be surprised. I had one in this write-up:
https://medium.com/@Muhammad_Wageh/idor-with-a-150-bonus-just-by-changing-patch-put-0e03b23c5b42
it was a PATCH Request, and I changed it to a PUT request and earned $150.
and also In this write-up: https://medium.com/@Muhammad_Wageh/500-in-just-a-single-idor-b13e9df22f4a
it was a PATCH Requst changed to a GET Request, and I earned $500.

    Just keep digging into your test until you find bugs and success.

# And this is from Bug Bounty BootCamp Book. I recommend to read it! Chapter 10 IDOR.

    Bypassing IDOR Protection:

Change the Request Method

If one HTTP request method doesn’t work, you can try plenty of others instead:
GET, POST, PUT, DELETE, PATCH, and so on. Applications often enable
multiple request methods on the same endpoint but fail to implement the same
access control for each method. For example, if this GET request is not vulner-
able to IDOR and doesn’t return another user’s resources
GET example.com/uploads/user1236-01.jpeg
you can try to use the DELETE method to delete the resource instead. The
DELETE method removes the resource from the target URL:
DELETE example.com/uploads/user1236-01.jpeg

If POST requests don’t work, you can also try to update another user’s
resource by using the PUT method. The PUT method updates or creates
the resource at the target URL:
PUT example.com/uploads/user1236-01.jpeg
(PUT request body)
NEW_FILE
Another trick that often works is switching between POST and GET
requests. If there is a POST request like this one
POST /get_receipt
(POST request body)
receipt_id=2983
you can try rewriting it as a GET request, like this:
GET /get_receipt?receipt_id=2983

# Happy Hacking💀

LinkedIn: https://www.linkedin.com/in/muhammad-wageh-55743a28a?utm_source=share_via&utm_content=profile&utm_medium=member_android

Hackerone:https://hackerone.com/muhammad_wageh_?type=user

medium:https://medium.com/@Muhammad_Wageh
