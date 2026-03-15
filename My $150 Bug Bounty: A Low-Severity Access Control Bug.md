Hey Everyone!

I just want to talk about a bug and a single validation that unfortunately makes the **severity Low**, and I got **$150**. Alright, so before talking about the bug, I actually spent **2 days** on the application just understanding it and taking notes (**Objects, Mechanisms, Attack Vectors, Validation, etc.**) and reading the **documentation**.

## Let's Start

The **User role** does not have access to the **Admin Center** — only the **Owner** and the **Admin** have access.

![Screenshot 1](https://github.com/muhammadwageh/write_ups/blob/main/images/user-.png?raw=true)  

![Screenshot 2](https://github.com/muhammadwageh/write_ups/blob/main/images/uset.png?raw=true)  


There is an important object responsible for **enabling and disabling verification**.

I intercepted the request with the **Owner account** and replaced two things:

Cookie: Session

Header: CSRF-TOKEN


with the **User-Role** values, and it worked — the user was able to **modify the action**.

![Screenshot 3](https://github.com/muhammadwageh/write_ups/blob/main/images/enable.png?raw=true)  
*Enable.*

![Screenshot 4](https://github.com/muhammadwageh/write_ups/blob/main/images/disable.png?raw=true)  
*Disable.*


![Screenshot 5](https://github.com/muhammadwageh/write_ups/blob/main/images/csrf.png?raw=true)  


But there was one thing: the **ID is UUIDv4**, unfortunately.

![Screenshot 6](https://github.com/muhammadwageh/write_ups/blob/main/images/uuid44.png?raw=true)  


When I tried to access the endpoint, it returned **all the UUIDs**, but unfortunately it responded with **403**.

![Screenshot 7](https://github.com/muhammadwageh/write_ups/blob/main/images/403.png?raw=true)  


So I reported it, and the triager told me it is **Low severity** because the **attack complexity involves UUIDv4**, and I got **$150**.

![Screenshot 8](https://github.com/muhammadwageh/write_ups/blob/main/images/triage.png?raw=true)  

![Screenshot 9](https://github.com/muhammadwageh/write_ups/blob/main/images/bounty.png?raw=true)

![Screenshot 10](https://github.com/muhammadwageh/write_ups/blob/main/images/2026-03-14_18-41.png?raw=true)  


---

**Thank you for reading!**
