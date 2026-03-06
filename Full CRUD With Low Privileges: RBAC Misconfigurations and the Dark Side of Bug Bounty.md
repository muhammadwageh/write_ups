# Hey everyone! I want to talk about several bugs I found:

* RBAC Misconfiguration Allows Low Privilege Role to Perform Full CRUD Operations
* RBAC Misconfiguration Allows Low Privilege Users Role to Update
* RBAC Misconfiguration Allows Low Privilege Role to Delete
* Some 403 bypass bugs that allow access to certain objects and closed information
* The dark side of bug bounty

And why was the CRUD reported as **Low severity**?
Why was the **update marked as duplicate (internal)**?
And why was the **delete marked as self-closed / N/A**?

Let’s start with the first one.

The **Low Privilege User** does not have access to the example object in the UI or the documentation. However, when I intercepted the request to **read, create, update, and delete** the object as the **Owner**, I replaced the session with the **Low Privilege User** role — and it returned **200 OK**.

I reported the issue. At first they told me it would be **Medium severity**, but the final decision was **Low severity**. So the only accepted bug was marked as **Low**, even though it allowed **full CRUD operations on a sensitive endpoint**.

<img width="347" height="749" alt="2026-03-06_21-48" src="https://github.com/user-attachments/assets/dbae6560-ad64-4912-8b6f-813ea7b491a2" />


Okay… let’s continue.

The second issue: **three users did not have permission to update a very important object**. I did the same thing — intercepted the request from the Owner and replaced the session cookie with tokens from the other three users. It still worked and returned **200 OK**.

When I submitted the report, they told me it was a **duplicate (internal)**.

<img width="957" height="283" alt="2026-03-03_15-05" src="https://github.com/user-attachments/assets/a974c9de-214a-4efe-9021-9698b43326dd" />


The third issue frustrated me even more. I created a **custom role with zero permissions**, intercepted the Owner’s request to **delete the object**, and replaced the cookie with the custom role session. It worked.

This endpoint did not require a user ID — it used the **object name**, which makes it easier to delete **multiple objects**. After replacing the cookie with the custom role session, the deletion worked.

But they responded: **“We don’t see any impact.”**

<img width="1175" height="318" alt="2026-03-03_15-07" src="https://github.com/user-attachments/assets/931114ef-30aa-4715-a274-5ba2e502de8e" />


How is there **no impact**?


Okay, let’s continue.

Finally, I found **three more bugs** where I accessed endpoints with **low privilege**. One of them allowed me to **bypass a 403 error simply by removing the ID from the request** and still access the endpoint.

Their response was that this is **expected behavior**, the information accessed is **not sensitive**, and they asked me to **self-close the report**. The others were marked as **Informational**.

---

## The Dark Side of Bug Bounty

This is sometimes the **dark side of bug bounty**. Internal teams manage the reports and sometimes **do not accept the bugs**, or they **downgrade the severity**.

I watched a talk by **Jason Haddix** about the dark side of bug bounty at DEF CON. It’s a long journey, and I’ve learned many things from it.

https://youtu.be/6SNy0u6pYOc?si=U67RupG8PhTD2rF8

When you choose a target, make sure it’s a **good company to work with**. Also ask other researchers if they have found bugs there.

I asked several researchers, and they told me they experienced the **same situation**.

It is what it is. I’m not complaining — we learn from our experiences. But it can be frustrating. Now, when I submit a report, I already expect it might be rejected.

Instead of thinking:
“Wow, I found a bug! Maybe I’ll get $1000.”

I just **submit the report and continue hunting**.

Thank you for reading!



