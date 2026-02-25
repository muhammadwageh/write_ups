# Hey everyone, I just want to talk about two Access Control issues I found.

They are related to **RBAC (Role-Based Access Control)**.

### Read Mechanism Issue

The first issue is related to a **read mechanism**. I was able to access an incident using a low-privilege user.

The low-privilege user cannot even access the incident from the UI.


<img width="897" height="669" alt="2026-02-24_22-06" src="https://github.com/user-attachments/assets/72e2e2b7-e935-4e0e-b346-375e6ebd9986" />


However, I captured the GET request used to retrieve the incident:

```
GET /api/v1/incidents/{incident_id}
```

The response was **200 OK**, which means the incident data was accessible through the backend API.


<img width="1648" height="760" alt="pic" src="https://github.com/user-attachments/assets/7a4849f8-fdfc-4de1-8258-447879d84ab1" />



When I reported it, the triager said the attack complexity is low because it would be difficult to enumerate or obtain all incident IDs. I explained that I reported it because it is a backend API misconfiguration.


<img width="1345" height="329" alt="2026-02-24_22-10" src="https://github.com/user-attachments/assets/5e8b5a62-cd4c-4d9d-b65f-7b232c0d7dda" />


In the end, the report was accepted but marked as **Low severity**.


<img width="289" height="717" alt="2026-02-24_22-01" src="https://github.com/user-attachments/assets/dca148c9-b914-4ad6-97fd-1e35b28c15ef" />


---

### Delete Draft Workflow Issue

The second issue is related to deleting draft workflows.


<img width="475" height="243" alt="Screenshot from 2026-01-17 20-41-47" src="https://github.com/user-attachments/assets/0f4ee60f-d246-49bc-9034-a9eddf4765e5" />


Low-privilege users cannot access workflows from the UI. However, I noticed there is a request related to a "draft" state, which allows editing the workflow after it has been made live.

When I attempted to delete the draft workflow using low-privilege users, the request worked successfully.


<img width="1654" height="724" alt="DELETE" src="https://github.com/user-attachments/assets/fb0a64f6-2d90-4120-8b3d-910a8e2a5ddf" />


The triager mentioned that since it only affects a draft, it would be considered **Medium severity**.


<img width="301" height="721" alt="2026-02-24_22-00" src="https://github.com/user-attachments/assets/2fc2da7a-8198-42f2-9445-5dc90d49f296" />


---

Finally, the internal team triaged the two bugs I found, and I received some nice swag 

<img width="1138" height="562" alt="bug" src="https://github.com/user-attachments/assets/ffc99ac9-4e94-4151-a158-b768ea5f0804" />




<img width="1140" height="589" alt="bug2" src="https://github.com/user-attachments/assets/12d9109e-d32c-4d61-9b40-df80ad196f55" />



Thank you for reading!

LinkedIn:
https://www.linkedin.com/in/muhammad-wageh-55743a28a/

