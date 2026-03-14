Hey Everyone!

I just want to talk about a bug and a single validation that unfortunately makes the **severity Low**, and I got **$150**. Alright, so before talking about the bug, I actually spent **2 days** on the application just understanding it and taking notes (**Objects, Mechanisms, Attack Vectors, Validation, etc.**) and reading the **documentation**.

## Let's Start

The **User role** does not have access to the **Admin Center** — only the **Owner** and the **Admin** have access.

![alt text](user-.png)

![alt text](uset.png)

There is an important object responsible for **enabling and disabling verification**.

I intercepted the request with the **Owner account** and replaced two things:

Cookie: Session
Header: CSRF-TOKEN



with the **User-Role** values, and it worked — the user was able to **modify the action**.

![alt text](enable.png)


![alt text](disable.png)


![alt text](csrf.png)

But there was one thing: the **ID is UUIDv4**, unfortunately.

![alt text](uuid44.png)

When I tried to access the endpoint, it returned **all the UUIDs**, but unfortunately it responded with **403**.

![alt text](403.png)

So I reported it, and the triager told me it is **Low severity** because the **attack complexity involves UUIDv4**, and I got **$150**.

![alt text](triage.png)

![alt text](bounty.png)

![alt text](2026-03-14_18-41.png)

---

**Thank you for reading!**
