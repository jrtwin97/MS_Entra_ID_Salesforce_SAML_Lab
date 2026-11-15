# MS_Entra_ID_Salesforce_SAML_Lab


<p>
  <img src="https://img.shields.io/badge/Microsoft_Entra_ID-0078D4?style=for-the-badge" alt="Microsoft Entra ID" />
  <img src="https://img.shields.io/badge/SAML_2.0-3076BE?style=for-the-badge" alt="SAML 2.0" />
  <img src="https://img.shields.io/badge/Salesforce-5089F7?style=for-the-badge" alt="Salesforce" />
    
</p>

I recently configured and implemented a real-world IAM solution by authenticating Salesforce with Microsoft Entra ID using SAML SSO. The goal is allow users to authenticate to Salesforce using their Entra credentials without needing separate passwords. SSO is more secure way to authenticate, the less passwords the better. 

Objectives

+	Configure Salesforce enterprise application for SAML Authentication
+	Define the application identifier and reply URL
+ Configure the attributes sent to the application
+ Assign user(s) to the application
+ Test the Authentication flow
  
Tools & Services Used

+	Microsoft Entra ID (Azure AD)
+	Salesforce Developer Edition
+	SAML 2.0 Protocol

<img width="894" height="473" alt="Screenshot 2026-11-10 195435" src="https://github.com/user-attachments/assets/fc628f52-0bad-499b-bb4a-3173f4757af6" />

1. Set Up the Salesforce App in Entra
+	Navigate to Entra ID > Enterprise Applications
+	Click “+ New Application” > Search and select Salesforce>Create
+	Under Single Sign-On, choose SAML
+	Enter the following values using your Salesforce subdomain: Confirm link has correct format if not URL will error red
+	Add Identifier (Entity ID) : https ://my.salesforce.com
+	Add Reply URL (ACS) : https ://mydomain.mysalesforce.com
+	Add Sign-on URL : https ://mydomain.mysalesforce.com

<img width="634" height="638" alt="Screenshot 2026-11-10 200418" src="https://github.com/user-attachments/assets/e30c6b30-5047-4d63-b787-7124742b523f" />

+	In SAML Certificates box> Edit > Download Federation Metadata XML from (Entra ID)

<img width="618" height="227" alt="Screenshot 2026-11-10 201058" src="https://github.com/user-attachments/assets/9faeb7ce-ae0a-4909-8753-a0dba772863a" />

Assign the Salesforce App to the Entra User

+	Go to Enterprise Applications > Salesforce > Users and Groups
+	Assign the user or group> Select Test User from tenant, no need to create new one
+	Assign Role > Standard User> Assign

<img width="1353" height="321" alt="Screenshot 2026-11-10 213048" src="https://github.com/user-attachments/assets/903cdb89-ad67-4d3b-b0e8-e5c35063ddae" />

2. Configure SSO Settings in Salesforce
+	Go to Setup > Single Sign-On Settings in Salesforce
+	On Salesforce setup page scroll down to Identity>Single Sign On Settings
+	Ensure SAML Enabled is checked
+	Single Sign On Settings> Click New Metadata File Upload XML File > Click Create
+	SAML Single Sign-On Settings Name>Entra ID>Save
+	Download Metadata File

<img width="1327" height="410" alt="Screenshot 2026-11-10 212010" src="https://github.com/user-attachments/assets/483cbeea-b570-48b8-9165-b7aa4ca2cf79" />

+	Open File in Notepad>Search for Entity ID URL Copy & Paste to another Notepad or Word Doc, Search Location ACS URL & repeat for previous step, Go back to SSO settings in Salesforce scroll down to Endpoints>Select Login URL Copy & Paste to previous step
+	Go back to Entra ID SSO Settings>Basic SAML Configuration replace each URL with Entity ID, Reply URL, & Sign On URL from Notepad or Word
+	Identifier (Entity ID): https://<your-subdomain>-dev-ed.my.salesforce.com
+	Reply URL (ACS): https://<your-subdomain>-dev-ed.my.salesforce.com
+	Sign-on URL: https://<your-subdomain>-dev-ed.my.salesforce.com
+	Go back to Salesforce>Company Settings>My Domain>Scroll Down to Authentication Configuration>Edit>Select Entra Id>Save

<img width="1340" height="271" alt="Screenshot 2026-11-10 212434" src="https://github.com/user-attachments/assets/62faf585-640d-4b74-9276-ccab7eedb1d7" />


3. Create Matching User in Salesforce
+	Go to Users > New User
+	Fill in required fields, then save
4. Test the SSO Flow
+	Navigate to Entra > Salesforce app > Single Sign-On > Test this application>Test Sign In

<img width="1585" height="380" alt="Screenshot 2026-11-10 213434" src="https://github.com/user-attachments/assets/c90a89c9-25e1-48df-822e-e9b38dc332b5" />

Test as another user open private browser





  Sign In

<img width="439" height="307" alt="Screenshot 2026-11-10 224103" src="https://github.com/user-attachments/assets/42376e2a-e1ac-437a-b2ee-885d8a3dc268" />



+	Redirects to Salesforce and receives Sign In Error

<img width="916" height="469" alt="Screenshot 2026-11-15 084254" src="https://github.com/user-attachments/assets/d15f48bf-c8d4-4ba3-89fe-52df4b8fe5cf" />

 
5. Issues I encountered and How I Solved Them

Researched Sign in Logs, to Test Entra ID which redirected in SAML SSO Settings found single logout checkbox was not enabled. After enabling, still received error. 

<img width="534" height="24" alt="Screenshot 2026-11-11 183440" src="https://github.com/user-attachments/assets/a7eb1676-b911-4f79-a391-3dab0826b928" />

 Also had user assigned to Salesforce enterprise app twice, one as user, also member of group. Removed group from enterprise app

<img width="1355" height="333" alt="Screenshot 2026-11-10 212807" src="https://github.com/user-attachments/assets/a5da0164-c5b1-4ba1-94da-1466c107e106" />

+	Reattempts to sign in using SSO
+	Sign in successful 
+ Navigate to new incognito window> go to myapps.microsoft.com> Sign in with test user tenant> Once authenticated select Salesforce

<img width="645" height="526" alt="Screenshot 2026-11-15 090616" src="https://github.com/user-attachments/assets/4ff840ee-5edf-42ae-8f1f-86a9fb490247" />

Login

<img width="589" height="268" alt="Screenshot 2026-11-10 213935" src="https://github.com/user-attachments/assets/2090bb45-e04b-4439-92f4-aaeae8821d8e" />

<img width="485" height="390" alt="Screenshot 2026-11-11 002026" src="https://github.com/user-attachments/assets/5bc7fc5c-a581-42cb-9f01-b72444ee0cfb" />

Successfully authenticated user through SSO Salesforce from Entra ID

6. I created a Conditional Access (CA) policy targeting the Salesforce app in Microsoft Entra. The policy specifically requires multi-factor authentication (MFA) whenever a user attempts to access Salesforce.

<img width="916" height="271" alt="Screenshot 2026-11-10 223141" src="https://github.com/user-attachments/assets/af401b54-ca50-49cf-8a13-52790ae45af7" />

This ensures:
+	Users can’t access Salesforce without verifying their identity
+	MFA is enforced regardless of location or device
+	The SSO flow is secure from token theft or basic phishing attempts

With SSO + Conditional Access in place, this setup aligns with modern zero trust principles while giving users a seamless login experience.

<img width="421" height="385" alt="Screenshot 2026-11-11 184033" src="https://github.com/user-attachments/assets/a584186a-916f-4d15-878d-49a0d4620c43" />

Skills Demonstrated
+	Microsoft Entra ID enterprise application administration 
+	SAML-based SSO configuration 
+	Application identifiers and reply URLs
+	Application access group assignment
+	Troubleshooting by researching logs


| Term | Definition | 
|---------|-------------|
| IdP (identity provider) | Microsoft Entra ID Authenticates the user and issues the signed SAML assertion. |
| SP (service provider) | Salesforce Trusts Entra and consumes the assertion. |
| Identifier (Entity ID) | The app's unique name must match exactly on both sides. |
| Reply URL / ACS URL | Where Entra posts the signed assertion. Must match exactly on both sides. |
| Application ID vs Object ID | The Application ID is the same in every tenant. The Object ID is unique to each tenant's service principal.|


What I learned
+	SSO is a trust relationship. The IdP signs, the SP verifies, and a mismatched certificate, identifier, or reply URL breaks it.
+	Certificates are credentials with expiry dates. Each Enterprise app is effectively a non-human identity to inventory and monitor.
+	Accessing sign-in logs with error details what went wrong. Troubleshoot by finding which side broke Entra, or Salesforce.
+	Proper IAM implementation is essential for reducing security risk and maintaining strong governance across enterprise environments.














