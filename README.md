# ReBombas_SCDFXIBM
Hi there! We are a group of incoming year 2 students studying under the Renaissance Engineering Programme at NTU.

## a) Team Description

Our team consists of Mei Jie, Wan Yi, Daryl, Shaun, and Julian! We have diverse interests and different specialisations, but are united in our desire for change.
## b) Pitch Video

You can find our video [here](https://pip.pypa.io/en/stable/)!

## c) Architecture of our proposed solution

Our architecture consists of 3 main sections, the device/sensor suite at each chute system, the cloud services, and the integration with our community.

![architecture](https://user-images.githubusercontent.com/60395624/84583784-3854d100-ae2f-11ea-899c-cad531973edd.jpg)

## d) Link to detailed solution
You can find our detailed report [here]()! Included in the document is our project roadmap/proposed timeline.

## e) Project Roadmap
Check out our 2020 Roadmap below:
![Roadmap.pdf](https://github.com/jyhw123/ReBombas_SCDFXIBM/files/4776075/Roadmap.pdf)

## f) Getting Started

### Log in to your IBM Cloud account
You'll need an IBM Cloud account to create Cloudant Databases, start up the IBM Watson Internet of Things Service, as well as launch a Node-RED app.

### Creating a Cloudant Database to store Chute addresses
First, we'll need to create a Cloudant Database and load a sample dataset of sensor IDs and their corresponding physical locations in Singapore. We'll use this database to send the addresses of rubbish chutes that are on fire to members of the public!

#### Step 1: Create an instance of a Cloudant Database Service
Head to 'Catalog' in the top navigation bar and search for Cloudant. Choose Dallas as the region, and name the instance: "Cloudant-TroubleChute". Leave the rest of the options as their defaults, and create the instance.

#### Step 2: Create a Database of Device IDs and Addresses
Go to your resource list and click on the Cloudant-TroubleChute service, and then click on the 'Launch Dashboard' button. Once on the Cloudant Dashboard platform, click on 'Create Database' on the top navigation bar. Name this database "troublechute_id_address", and choose Non-partitioned, then create. 

#### Step 3: Add an entry into the Database
Create a new doc by clicking on the '+' icon next to 'All Documents'. Then, delete any entries in the document, and the code below into the document:
```bash
{
  "_id": "TempSensor_1",
  "address": "1 North Bridge Road"
}
```
#### Step 4: More entries
Repeat step 3, two more times, with the two code blocks below:

```bash
{
  "_id": "TempSensor_2",
  "address": "13 Farrer Park Rd"
}
```
```bash
{
  "_id": "TempSensor_3",
  "address": "Block 420 HDB Clementi"
}
```
Your troublechute_id_database should now have 3 documents in it. If this project goes into production, this database would include the ID's of all Rubbish Chutes in HDB flats, with their corresponding physical addresses. 

#### Step 5: Get Service Credentials
Go to Resource List -> Services, and click on the Cloudant service that was started. Click on the ‘Service credentials’ side tab, there should be a set of credentials created. Click on the copy icon next to it, and paste the output on a text editor. The result should look like this:

```bash
{
  "apikey": "bq7Y5Yp1_g8_ku0JaoJuKpypfmjrYwTLAMgqu68ZCSlU",
  "host": "a88c241a-abbb-4e63-b84c-6d744abc030c-bluemix.cloudantnosqldb.appdomain.cloud",
  "iam_apikey_description": "Auto-generated for key dce751b5-fa9f-4e46-9f17-869c9e781d21",
  "iam_apikey_name": "f1a8c065-ec43-4414-9946-215d7a9bac7c",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/9a451e4dcf4c4bcf8bbeafe9a81cbfb8::serviceid:ServiceId-12753bc4-d12b-4929-9ab5-fcf34cb225ae",
  "password": "2ff52c1f65a2ba9a674b0028608256bec1161a11afbc6e020cc9eb1a0c0d4a68",
  "port": 443,
  "url": "https://a88c241a-abbb-4e63-b84c-6d744abc030c-bluemix:2ff52c1f65a2ba9a674b0028608256bec1161a11afbc6e020cc9eb1a0c0d4a68@a88c241a-abbb-4e63-b84c-6d744abc030c-bluemix.cloudantnosqldb.appdomain.cloud",
  "username": "a88c241a-abbb-4e63-b84c-6d744abc030c-bluemix"
}
```
Note down “host”, “password”, and “username”. We will need these credentials to ensure that the simulated device data can be stored in the Cloudant database via the Node-RED flow.

### Start a simulated device on IBM Watson IoT Platform
We will create a device to simulate readings being sent from the temperature and smoke sensors at a chute. 

#### Step 1: Create simulated device
Return to the IBM Watson IoT Platform. Create a simulated device by going through the tutorial [here](https://cloud.ibm.com/docs/IoT?topic=IoT-sim_device_data#sim_device_data). Device type should be "TempSensor". Instead of specifying a event payload, upload a CSV file called "data_final.csv" included in this repo. Ensure to enable the "Loop simulated data from file" option. The schedule should be 1 every minute, but can be set to a higher frequency for testing purposes. Then press "Create Simulated Device".

#### Step 2: Create API Key and Token
Go to "Apps" in the sidebar, and then click on "Generate API Key". Set Role as "Device Application", then create the key, taking down the key and the token. 

### Create Twitter Developer Account
Proceed to this video [here](https://www.youtube.com/watch?v=vlvtqp44xoQ&list=LLeYMU2wNabnzmnxyOodTStA&index=2&t=0s), and take note of the API Key, API Secret Key, Access Token, and Access Token (secret). Choose any Twitter ID you wish and note it down.

### Create a Node-RED application connected to an IoT Service
This is where we'll import our Node-RED flow from the .json file included in our repo. 

#### Step 1: Start a Node-RED application
On your IBM Cloud Platform, click on catalog, and then search for the 'Node-RED App' tile. Name it "Node RED TroubleChute app", and under Service Details - Pricing Plan, select the existing Cloudant Instance, "Cloudant-TroubleChute". Ensure tat the region should be Dallas. Click Create. 

#### Step 2: Create an Internet of Things Platform Service
Click on 'Catalog', then search for the Internet of Things Platform tile. Once there, ensure the region is Dallas, then name the service "Internet of Things Platform-TroubleChute". Then, press create. Then, press 'Launch' to open a window with the IBM Watson IoT Platform, which we will navigate back to later.

#### Step 3: Connect IoT Service to app
Go to Resource List -> Apps -> Node RED TroubleChute app. Then, press 'connect existing services' to connect to the "Internet of Things Platform-TroubleChute" service

#### Step 4: Deploy your Node-RED app.
Head back to Resource List -> Apps -> Node RED TroubleChute app. Then, click on "Deploy your app". Press 'New' to create a new IBM Cloud API key. Leave the memory allocation as 128mb if on lite account, ensure region is Dallas. Click next, then ensure the DevOps toolchain name is "NodeREDTroubleChuteapp", with region as Dallas, and proceed. After a few moments, the status should be In Progress. Wait for the deploy stage to finish running. 

#### Step 5: Update dependencies on the package.json file and install nodes
Proceed to the App Source/Git page to edit the package.json file. Under the dependencies section, add the following three dependencies:

```bash
{
...,
"node-red-contrib-scx-ibmiotapp": "0.x",
"node-red-dashboard": "2.x",
"node-red-node-twitter": "1.x"
}
```
Then, go to Manage Palette at the navigation bar, and press import, install the following node: 
```bash
node-red-node-ui-table
```
Now, we have all the required nodes/modules for the flow.

#### Step 6: Import our Node-RED Flow
Return back to Resource List -> Apps -> Node RED TroubleChute app. Open the Node-RED editor and secure it. Import the final_overall_flow.json file saved in this repo into the Node-RED editor. 

#### Step 7: Ensure that all nodes have the correct settings
In order for the simulated device to send data over to the Node-RED Flow, Cloudant, and Twitter we need to set a few settings.
##### IBM IoT App In Node
Authentication mode should be API Key. Edit the TempSensor_API API Key and input the API Key and Token generated above on the IBM Watson IoT Platform. The Device Type should be set to “TempSensor”, and the Device Id should be set to “TempSensor_1”, in accordance to the simulated device created a few steps ago.
##### Cloudant Nodes
We will need the service credentials for the Cloudant Database that was generated a few steps above. After clicking on the “Cloudant Address Retrieval” node, follow the following steps: ‘Service’ field should be set to ‘External cloudant or couchdb service’. Edit the “Server” field, and input the host field as “https://<your host name generated above>”, input the Username field as the username generated above, and the password field as the password generated above. Then, the Database name should be “troublechute_id_address”, and we should search by _id. There are 2 Cloudant Address Retrieval nodes that need to be edited as above.
  
##### Twitter Nodes
For each of the two Twitter nodes, we will need to add new twitter-credentials, then enter the created Twitter ID with the API Key, API secret key, Access token, and Access token secret generated above

### Functionality
If all the above steps are completed correctly, the simulated device should now be sending data to the Node-RED flow. This data can then be visualised using the Node-RED dashboard, and is also stored in the two Cloudant databases, “yesfire” and “nonfire” which should be automatically created under the Cloudant platform. When the data being sent indicates a fire in a chute device, a tweet will be sent out automatically, along with directions via Google Maps to the fire. The data could then be sent to the MyResponder App as well. 
