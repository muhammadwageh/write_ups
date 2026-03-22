Hey Everyone!

I just want to talk about a bug and a single validation that unfortunately makes the **severity Low**, and I got **$150**. Alright, so before talking about the bug, I actually spent **2 days** on the application just understanding it and taking notes (**Objects, Mechanisms, Attack Vectors, Validation, etc.**) and reading the **documentation**.

## Let's Start

The **User role** does not have access to the **Admin Center** — only the **Owner** and the **Admin** have access.

<img width="1519" height="313" alt="user-" src="https://github.com/user-attachments/assets/2c754819-c0e5-4c9c-b7b5-948b2d020126" />

<img width="1527" height="696" alt="uset" src="https://github.com/user-attachments/assets/f5e5d4a7-7ef4-4eb5-834e-76249f5aafcc" />



There is an important object responsible for **enabling and disabling verification**.

I intercepted the request with the **Owner account** and replaced two things:

Cookie: Session

Header: CSRF-TOKEN


with the **User-Role** values, and it worked — the user was able to **modify the action**.

<img width="1669" height="768" alt="enable" src="https://github.com/user-attachments/assets/f027f7d1-63bb-4127-999a-17daea842fa0" />

*Enable.*

<img width="1682" height="766" alt="disable" src="https://github.com/user-attachments/assets/4fc9f33a-7fff-473b-874c-7620d4e36c81" />
 
*Disable.*


<img width="1675" height="787" alt="csrf" src="https://github.com/user-attachments/assets/a4e4cb01-170c-4c62-9aec-97463d363313" />



But there was one thing: the **ID is UUIDv4**, unfortunately.

<img width="583" height="192" alt="uuid44" src="https://github.com/user-attachments/assets/bfe6892b-9162-4c4b-9b48-09a6138ec17d" />
 


When I tried to access the endpoint, it returned **all the UUIDs**, but unfortunately it responded with **403**.

<img width="1866" height="794" alt="403" src="https://github.com/user-attachments/assets/648aacc8-346e-4434-b601-d64ef01e544b" />



So I reported it, and the triager told me it is **Low severity** because the **attack complexity involves UUIDv4**, and I got **$150**.

<img width="873" height="188" alt="triage" src="https://github.com/user-attachments/assets/c163b071-f1ec-492f-ba52-2ad4ab1987b8" />


<img width="884" height="284" alt="bounty" src="https://github.com/user-attachments/assets/6f35b52c-eb38-4f46-9103-2ab7817c6f74" />

<img width="324" height="748" alt="2026-03-14_18-41" src="https://github.com/user-attachments/assets/e890bf0f-23a5-4893-8f1e-59da3efdd744" />



---

**Thank you for reading!**
