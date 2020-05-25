1.		Login on https://misplab.francecentral.cloudapp.azure.com with credentials provided M10d6nj!M10d6nj!
 
2.		The next step is to add the Microsoft feed to the MISP server. Click on “Sync Actions” pn the top menu  “List Feeds”
 


3.		Click on “Add feeds”
 
4.		•	Enter the address of Microsoft’s COVID-19 feed: https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Sample%20Data/Feeds/Microsoft.Covid19.Indicators.csv 
•	Select “Enabled”
•	Name: “Team X - Microsoft COVID-19 Indicators”  X is your team’s ID (letter)
•	Provider = Microsoft
•	Input Source = Network
•	Source Format = Simple CSV Parsed Feed
•	Creator Organisation, select Team X  X is your team’s ID (letter)
•	Set the ‘Value field(s) in the CSV’ to 2
•	Select “Auto Publish”
•	Distribution = Your organisation only
•	Add
 
 

There are several other 3rd party feeds you may also want to enable and have available in your Sentinel workspace. Each of these will need to be enabled separately.
5.		The next step is to ensure that the feed is automatically updated. In the ‘Scheduled Tasks’ section of the Administration menu on the top set the fetch_feeds task frequency to 1h.

6.		Retrieve your MISP auth key.
Within the MISP web interface click ‘Event Actions’ on the menu bar then select ‘Automation’. Your MISP auth key will be listed on the screen, note this down for entry into the script later.
 
MISP Auth Key is the red string.

Task 2: Create an App Registration with the required permissions
Step	Action
1.		Login on https://portal.azure.com with credentials provided

2.		Search for Azure Active Directory service
 
3.		Create new app registration. Azure AD  App Registration  + New Registration
 

4.		Insert Name: “MISP_UserX”, select “Accounts in this organizational directory only (Format only - Single tenant)” and click Register
 
5.		take note of Application ID and Tenant ID
 
6.		Under API permissions, choose Add a permission > Microsoft Graph. 
7.		Search anc click on “WindowsDefenderATP”
 
8.		Under Application Permissions, add ThreatIndicators.ReadWrite.OwnedBy.
 
9.		Note message: “You are editing permission(s) to your application, users will have to consent even if they’ve already done so previously.” 
That means Global Admin consent is required
10.		Send message on General Teams channel with your Team Name asking for consent.
Tutors will apply Consent to you App Registration
11.		Create a new client secret in “Certificate” & Secrets”
 
12.		Click “+ New client secret”  Add Description “MISP_UserX”, Select 1 year and click Add
 
 
13.		Copy Value on your text editor.

Task 3: Enable Azure Sentinel Connector

Step	Action
1.		Login on https://portal.azure.com with credentials provided and open your Azure Sentinel Workspace. 
•	https://portal.azure.com
•	Search for “Azure Sentinel”
•	 
•	Select your workspace  SentinelTeamX
•	 •	 
2.		Click on Data Connector
  

3.		click ‘Data connectors’ and then look for the ‘Threat Intelligence Platforms’ connection. Open the connector
 
4.		Click “Connect”
 
5.		Click “Next Steps” “Threat Intelligence” under Recommended workbooks
  
6.		
7.		

Task 4: Setup the script
Step	Action
1.		Using putty (or your preferred SSH client) logon to: misplab.francecentral.cloudapp.azure.com using credential provided
 

2.		Enter the following commands. These will create an environment for the script to run, download it from GitHub, install the necessary prerequisites and open the configuration file. 
sudo apt-get install python3-venv
python3 -m venv mispToSentinel
cd mispToSentinel
source bin/activate
git clone https://github.com/microsoftgraph/security-api-solutions
cd security-api-solutions/Samples/MISP/
pip install -r requirements.txt
nano config.py
3.		There are a few options that need to be changed in the configuration file:
•	Under the graph_auth key enter the details from the AAD App Registration earlier.
•	Set the ‘<targetProduct>’ to be ‘Azure Sentinel’.
•	I added a # comment at the start of each line in the misp_event_filters section to effectively disable any filtering, all data from the MISP server will be available in Sentinel.
•	Set ‘<action>’ to ‘alert’.
•	Enter you MISP auth key in ‘<misp key>’ and URL in ‘<misp url>’.
•	Finally set the lifetime for this data, I would recommend 30-60 days depending on your use case.
 
Ctrl + X  Yes
4.		You can now run the script to pull data from the MISP instance and push into your Sentinel workspace.
python script.py
 

Task 5: Use the data
Step	Action
1.		Login on https://portal.azure.com with credentials provided and open your Azure Sentinel Workspace. 
•	https://portal.azure.com
•	Search for “Azure Sentinel”
•	 
•	Select your workspace  SentinelTeamX
•	 
2.		After a few minutes you should be able to query the ThreatIntelligenceIndicator table in your Sentinel workspace.
Click on “Logs” section and type search:
ThreatIntelligenceIndicator
| count 

