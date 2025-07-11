### From Informative To Medium (6.5) How I Found Two Insecure Direct Object Reference (IDOR) vulnerability In Desktop Application ? it's allows me to change the user name and the job title and access all sensitive data.
## First Learn The App 
The best programs to do logic testing on are Software-as-a-Service (SaaS) companies that have large, complex web applications. The application must have authentication and should have complex access controls, a wide range of mechanisms/functionality, and designed to be used by a large number of users simulatenously. Unlike injection testing where you can isolate specific attack vectors, logic testing requires requires the bug bounty hunter to see the "bigger picture" of how the different components of an application work together. You must first understand how the app works so you can identify the cracks and try to bend/break the logic. Before I begin logic testing a target application, I spend at least 2-3 days understanding how the application works before I do ANY testing. Your mindset should be that you have been hired by this company and are now part of their application security team.
### Taking Note:

### Accounts:

1. demouser1@com  -> Desktop App  
      `"id":"123"`
      teams :
            - demouser1 team1
                  - Id: `789`
            - demouser1 team2
                  - Id: `876`

2. demouser2@.com  ->  Browser     
      `"id":"456"`
        teams :
            - demouser2 team1
                  - Id: `345`
                  - demouser1@.com {Can Edit}
            - demouser2 team2
                  - Id: `654`
                  - demouser1@.com {Can View}

### Objects

- User
- Team
- Project
- File


### Machanism

User 
      - Change Name 
      - Change Email 
      - Change Role
      - Change Password
      - Change Two-Factor Auth
      - Change Language
      - Upload Avatar
      - Enable Libraries 
      - Personal Access Tokens

Team 
      - Create Team
      - Rename Team 
      - Delete Team
      - Change Icon


Project
     - Create Project
     - Add Description
     - Move Project
     - Rename Project
     - Transfer Project
     - Delete Project



File 
     - Create File
     - Share File
     - Move File 
     - Delete File 

### Application Boundaries 

1. User -> User
      - IDOR
2. Team (Can View) -> Team (Can Edit)
      - RBAC
3. Team (Can Edit) -> Team (Admin)
      - RBAC
4. Project (Can View) -> Project (Can Edit)
      - RBAC

### Validation Patterns 


#### Validation Session

Two Values in Request Are Required: 
      - cookie: `__Host-.authn=%7B%userid=123`
      - Header : `User-Id: 123` 

#### Validation IDORs

PUT -> Result : 403 You don't have permission to do that. Contact your team administrator

#### Validation RBAC

PUT -> Result : 403 You don't have permission to do that. Contact your team administrator

### Attack Vectors 
- I Found the User-id In Four Place 
    Body 
    Cookie 
    header 
    Referer

# First Bug Change user name 

PUT -> https://www.examble.com/api/user

`"name":"demouser1test","id":"123"`

`{"123":".authn.abc"}`
`Header-User-Id: 123`
`eferer: https://www.examle.com/files/team/123/all-projects?fuid=123`

      - Machanism: Update User Name 
      - CRUD: Update
      - Object: User
      - Boundary: User -> User (IDOR)
      - Unique Identifier: "id":"123" (?)
            - Are the id number sequencial ?
                  - No, only digital, some pattern (15176)
            - Is the id value used to pull the larger user object ?
                  - Yes. unless they use the unique token at `figtkn.3733153970.authn.abc`
            - How does session come into play ?
                  - Not at all, cookie isn;t needed, unless `figtkn.3733153970.authn.abc` is the session token 
            Tests: Updating The Name 
                  - test 1 - Works, Changes demouser2
                        Cookie 2
                        Referer 2
                        X-header 2
                        Id kvp 1
                   - test 2 - Works, Changes  `demouser1` name  (Valid Bug)
                        Cookie 2
                        Referer 2
                        X-header 1
                        Id kvp 1
            Result:
                  - the X--User-Id header is important for validation request to modify the User object

### Steps To Reproduce:

1. [Log in to the  web application (https://www.examble.com) with an attacker account..] 
2. [Log in to the  desktop application with a victim account.]
3. [In the desktop application, inspect network traffic (e.g., using a proxy or developer tools) to obtain the victim’s X-header-User-Id (a 19-digit number, e.g., 12345xxxxxxxxxxxxxxxx).] Intercept the Request:
4. [In the web application, navigate to profile settings and initiate a name change.]
5. [Use Burp Suite to intercept the HTTP request sent to the name change endpoint (e.g., PUT /api/user).] Modify the Request: 
6. [In the intercepted request, locate the X-heade-User-Id header (e.g., X-header-User-Id: attacker_user_id).]
7. [Replace it with the victim’s 19-digit user ID obtained from the desktop application (e.g., X-header-User-Id: 12345xxxxxxxxxxxxxxxx).]
<img width="1452" height="738" alt="2025-07-10 23_01_40-changeid" src="https://github.com/user-attachments/assets/088bef5c-3147-4348-b217-ed75cba65d58" />
<img width="1450" height="756" alt="2025-07-10 23_00_29-firstResponse" src="https://github.com/user-attachments/assets/badbf239-a075-4460-a99c-abe1358618a1" />

9. [Update the request body to set a new name (e.g., {"name": "New Malicious Name"}).] Submit the Request: 
 Forward the modified request to the server. The server processes the request, updating the victim’s name and returning profile data in the response. Verify the Exploit:
10. [Log in to the desktop application with the victim account or check a shared header workspace to confirm the name has changed to “New Malicious Name.”]
11. [Review the API response in Burp Suite to verify exposed data (e.g., email, profile metadata).]
12. <img width="1372" height="578" alt="2025-07-10 23_04_03-secondRespons" src="https://github.com/user-attachments/assets/78f174f8-6f31-4456-bcbb-61d5cf99fde0" />


-----------------------------------------------------------------------------------------------------------
# Second Bug Change the job title 

PUT -> https://www.examble.com/api/user

`"name":"demouser1test","id":"123"`
      tests : Updating The Role Title it works Change the demouser1 `role title `
                  - Header - userid=123

in the request body i Added the header name and it's working in the single request when i change the role title i can change the user name and the role job tile 


### Steps To Reproduce:
1. [In the web application, navigate to the settings or team management section where role titles can be updated (User profile).]
2. [Initiate a role title change (e.g., set a new role like “Software”).]
3. [se Burp Suite to intercept the HTTP request to the role title update endpoint (e.g., PUT /api/user).] Modify the Request:
4. [Locate the X-header-User-Id header in the intercepted request (e.g., X-header-User-Id: attacker_user_id).]
5. [Replace it with the target’s 19-digit user ID from the desktop application (e.g., X-header-User-Id: 12345xxxxxxxxxxxxxxxx).]
6. [Update the request body to set a new role title (e.g., {"role_title": "Software"}).] Submit the Request:
7. <img width="771" height="541" alt="2025-07-10 23_05_58-updateRole mp4 - VLC media player" src="https://github.com/user-attachments/assets/4048d657-fdcd-40a9-95bb-1bc72ab59ab1" />

8. [Forward the modified request to the server.]
9. [The server processes the request, updating the target’s role title and potentially returning profile data.]
10. [add step] Verify the Exploit:
11. [Check the target user’s profile or role in a shared  workspace (via desktop application) to confirm the role title has changed to “Software.”]
12. <img width="716" height="538" alt="2025-07-10 23_08_25-changenamewithjason mp4 - VLC media player" src="https://github.com/user-attachments/assets/070267c4-fad2-44c7-a552-a75a341722f0" />

13. [Review the API response in Burp Suite for any exposed data (e.g., user profile details).]
14. Following my previous submissions I’ve discovered an enhanced exploit for the IDOR vulnerability. By modifying the JSON body of the profile update request to include both "name" and "role_title" (e.g., {"name": "New Malicious Name", "role_title": "Admin"}), I can change a target user’s name and role title in a single request using the manipulated X-header-User-Id header (19-digit ID with fixed 5-digit prefix). This amplifies the impact, enabling impersonation of privileged roles (e.g., Admin) alongside name changes, logging into the web app with an attacker account, intercepting the request, modifying the X-header-User-Id to a victim’s ID from the desktop app, updating both name and role title, and verifying the changes.
15. <img width="813" height="520" alt="2025-07-10 23_09_43-changenamewithjason mp4 - VLC media player" src="https://github.com/user-attachments/assets/9e1fd458-1cc9-49b0-a7aa-2eb099bd85e4" />


<img width="314" height="800" alt="2025-07-11 02_44_01-Settings" src="https://github.com/user-attachments/assets/e36e16cb-9eed-4d68-bdbb-6f73d6239c79" />


https://github.com/user-attachments/assets/07870fde-b178-46c8-bfd5-2795aafe72f2




### References
 
 1. https://github.com/R-s0n/bug-bounty-village-defcon32-workshop/blob/main/logic-methodology.md
 2. https://youtu.be/BfbS8uRjeAg?si=lRjCD__9D0O6TSsm
 3. https://youtu.be/4h42AFrpyK0?si=W5Mwb_iD2q1RjBUe
 4. https://youtu.be/jTdqM2aO4Ys?si=ENzThnxaGjG34y54
 5. https://youtu.be/EeBSqo7N2Bs?si=sgGXS7zI7XrgReTv

 ### Contact 
 1. https://www.linkedin.com/in/muhammad-wageh-55743a28a/
 2. https://medium.com/@Muhammad_Wageh
 3. https://github.com/muhammadwageh

 
