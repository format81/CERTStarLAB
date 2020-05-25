Step	Action
Login on https://portal.azure.com with credentials provided

Search for Azure Active Directory service
 
Create new app registration. Azure AD  App Registration  + New Registration
 

Insert Name: TPI_UserX, select “Accounts in this organizational directory only (Format only - Single tenant)” and click Register
 
take note of Application ID and Tenant ID
 
Inside Azure AD app just created  API Permissions  +Add a permission and select “APIs my organization uses”
 
Search anc click on “WindowsDefenderATP”
 
Click on Application Permissions  search and Select “Ti.ReadWrite - Read and write IOCs belonging to the app”. That means application will use MDATP API to read and write IoCs.  Click on add permissions
 
You will receive a notification on top saying: “You are editing permission(s) to your application, users will have to consent even if they’ve already done so previously.” 
That means Global Admin consent is required. 
Please send a message on General Teams channel with your Team Name asking for consent.
Tutors will apply Consent to you App Registration remotely upon your request. Please note that this step is mandatory and necessary to proceed further.
Create a new client secret in “Certificate” & Secrets”
 
Click “+ New client secret”  Add Description “TPI_UserX”, Select 1 year and click Add
 

Copy the new client secret Value on your text editor.


1.		Login on https://minemeld.formato.info with credentials provided

2.		Descrizione Miners, ecc

3.		Configure Miners to aggregate Threat Indicators. 
Click Config  click on “Eye” icon on the left down hand side of the screen , a + button will appear  Click on it
 
4.		Name “demouserX”  Prototype “microsoft_wd_adtp.outputBatch” (don’t worry about the alert)  Inputs “inboundaggregator”  click OK
 

5.		Click “Commit” and wait for COMPLETE services restart.
6.		Edit MDATP node properties in “Nodes” → click on the node just created, scroll to “Settings” section and provide: CLIENT ID (Azure AD Application ID), CLIENT SECRET: (Client Secret), TENANT ID (Azure AD Identifier)
 

 

 
7.		Insert TeamX’s indicators. 
Note: MineMeld do not push duplicated indicators. In order to see your own IOC implement below procedure.

MineMeld  Node  Local IOC –> select 3rd tab on the left “Indicators” and click on + Add Indicator
Insert you indicator i.e. 123.123.123.123  Type IPv4  Comment insert your account name “demo.userX”. Click OK
Comment: TeamX (it could be useful for troubleshooting purpose if needed)
 
8.		Please chat custom IOC pushed to General MS Teams channel chat
 
1.		Login on https://securitycenter.windows.com with credentials provided

2.		Open the hamburger menu and expand “Settings” In “Rules” section click on “Indicators”  IP Addresses
 

Notice “Created by” field. You can Filter on this fied to see your IoCs
3.		Click on Filter and search for you custom IOC
 

 

