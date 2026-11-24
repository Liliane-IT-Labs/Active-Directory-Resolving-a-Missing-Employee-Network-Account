# **Active Directory | Resolving a Missing Employee Network Account**

## **1. Employee Complaint**

> "I'm a new employee and I don't have a network account."

A newly hired marketing manager, Juan Suarez, contacted the IT Help Desk because he did not have an account to access the corporate network.

My objective is to investigate the complaint, identify the root cause, create the missing Active Directory account, and verify that the employee can successfully sign in.

## **2. Support Ticket**

| Field           | Details                              |
| --------------- | ------------------------------------ |
| Employee        | Juan Suarez                          |
| Department      | Marketing                            |
| Job role        | Marketing Manager                    |
| Category        | Account Management                   |
| Request type    | Service Request                      |
| Issue           | Missing network account              |
| Business impact | Employee unable to begin work        |
| Tool            | Active Directory Users and Computers |
| Domain          | CorpNet.local                        |

## **3. Troubleshooting Workflow**
<img width="633" height="681" alt="image" src="https://github.com/user-attachments/assets/02f7013c-c26a-426e-b9a4-55e4f1138d0f" />

## **4. Step-by-Step Resolution**

### **Step 1 — Verify employee information**

**Goal:** Confirm that the employee is authorized to receive a corporate network account.

Review the onboarding information provided by Human Resources.

| Information         | Verified details            |
| ------------------- | --------------------------- |
| First name          | Juan                        |
| Last name           | Suarez                      |
| Department          | Marketing                   |
| Position            | Marketing Manager           |
| Organizational Unit | Marketing\MarketingManagers |
| Username            | jsuarez                     |

### **Step 2 — Investigate the missing account**

**Goal:** Determine whether the account exists before creating a new one.

1. Launch the **CorpDC** virtual machine.
2. Open **Server Manager**.
3. Navigate to **Tools → Active Directory Users and Computers**.
4. Right-click `CorpNet.local`.
5. Select **Find**.
6. Enter `Juan Suarez`.
7. Click **Find Now**.

Alternatively, search using PowerShell:

```
Get-ADUser -Filter "SamAccountName -eq 'jsuarez'"
```

**Expected finding:** No existing account is found.

**Root cause:** The employee's Active Directory account has not been provisioned.

📸 Screenshot 01 — Active Directory account investigation.

### **Step 3 — Create the missing network account**

**Goal:** Provide the employee with a unique corporate network identity.

Navigate to:

```
CorpNet.local
└── Marketing
    └── MarketingManagers
```

1. Right-click **MarketingManagers**.
2. Select **New → User**.
3. Enter the employee's information.

| Field           | Value          |
| --------------- | -------------- |
| First name      | Juan           |
| Last name       | Suarez         |
| Full name       | Juan Suarez    |
| User logon name | jsuarez        |
| Domain          | @CorpNet.local |

4\. Click **Next**.

📸 Screenshot 02 — New Active Directory user account.

### **Step 4 — Configure the account password**

**Goal:** Secure the employee's account before granting network access.

Enter the authorized temporary password and configure the following settings:

- User must change password at next logon
- User cannot change password
- Password never expires
- Account is disabled

Click **Next → Finish**.

This ensures the employee must create a personal password during the first login.

📸 Screenshot 03 — Password configuration.

### **Step 5 — Verify account creation**

**Goal:** Confirm that the account exists, is enabled and is located in the correct Organizational Unit.

Run the following PowerShell command:

```
Get-ADUser -Identity jsuarez |
Select-Object Name,SamAccountName,Enabled,DistinguishedName
```

Verify the following:

| Verification    | Expected result   |
| --------------- | ----------------- |
| Account exists  | Yes               |
| Username        | jsuarez           |
| Account enabled | True              |
| Departmental OU | MarketingManagers |

📸 Screenshot 04 — Active Directory account verification.

### **Step 6 — Test employee login**

**Goal:** Confirm that the original employee complaint is resolved.

From a domain-joined workstation:

1. Select **Other user**.
2. Enter `jsuarez@CorpNet.local`.
3. Enter the temporary password.
4. Change the password when prompted.
5. Confirm successful login.

Open Command Prompt and run:

```
whoami
```

Verify that the authenticated identity belongs to the corporate domain.

📸 Screenshot 05 — Successful domain authentication.

## **5. Ticket Resolution**

| Field             | Details                                     |
| ----------------- | ------------------------------------------- |
| Root cause        | Missing Active Directory account            |
| Corrective action | Created and configured the employee account |
| Security measure  | Mandatory password change at first login    |
| Verification      | Successful domain authentication            |
| Ticket status     | Closed after employee confirmation          |

**Resolution notes:**

> Verified the employee's onboarding information with HR, identified the missing Active Directory account, provisioned the account in the MarketingManagers OU, and configured mandatory password change at first login. Close the ticket once successful authentication is confirmed.

## **6. Technical Skills Demonstrated**

- Help Desk troubleshooting
- Active Directory administration
- User account provisioning
- Identity and Access Management (IAM)
- Organizational Unit management
- Password security
- PowerShell
- Domain authentication

## **7. Business Impact**

Resolving a missing network account allows a new employee to begin working while maintaining corporate security and compliance with account provisioning policies.

