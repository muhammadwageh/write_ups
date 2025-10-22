# First, I spent two days understanding the target and taking notes on things like:
- Objects
- Mechanisms
- Application boundaries
- Roles
- Validation patterns
  - Validation for IDORs
  - Validation for RBAC
  - Validation for sessions
- Attack vectors

I noticed that the Member role and Collaborator role are not supposed to have access to the Meta-Data field.

Owner Role:

<img width="1585" height="802" alt="OwnerDATA" src="https://github.com/user-attachments/assets/4053072b-a1de-4c5f-ba49-74cd664adf05" />

Member Role:

<img width="267" height="453" alt="MemberRole-" src="https://github.com/user-attachments/assets/fd720fea-c379-42ed-8a6b-411f3dec84a2" />

Collaborator Role:

<img width="235" height="417" alt="Collaborator---" src="https://github.com/user-attachments/assets/74236c0d-0aa5-4356-8d85-595f12900aad" />


So, I tried different attack vectors like Create, Update, and Delete, but they weren't working. In this request:
## Create Data:
POST -> https://api.test.com/api/clm-metadata/workspace/
`{"key":"test","settings":{},"metadata_category":"custom","metadata_type":"single_line","is_removed":false}`
- Machanism: Create Data
- CRUD: Create
- Object: Data 
- IDOR Boundary: User -> User
- RBAC Boundary: workspace Non-Admin -> Admin 
- Tests:
    - Admin Create Data
      
        - Expecation: Success
      
        - Result: Success
      
     - Member Create Data
  
        - Expecation: Fail
       
        - Result:`{"type":"permissions_error","detail":"You do not have permission to perform this action."}`
       
     - Collaborator Create Data
        
        - Expecation: Fail
       
        - Result:`{"type":"permissions_error","detail":"You do not have permission to perform this action."}`
       
- I replaced the Authorization with the Member role and Collaborator

And the response was:
- Result:`{"type":"permissions_error","detail":"You do not have permission to perform this action."}`
  
I tried different bypasses from "https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-web/403-and-401-bypasses.html#403--401-bypasses" and changed the method from POST to GET, and BOOM—now I could see all the Data fields!

So, I tried it with two low-privileged roles: Member and Collaborator. It worked, and I reported them as two vulnerabilities, one for each role.

Unfortunately, it was a duplicate!

<img width="400" height="663" alt="RBAC" src="https://github.com/user-attachments/assets/36a08f58-4206-4d71-a7aa-85045534f38e" />


See you in the next valid bug report.

Cheers,  
Muhammad Wageh  
