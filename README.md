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
You can find our detailed report [here](https://docs.google.com/document/d/1numWPoFQDbeBCx7jjGLD8Cilfhq-Mh5aV4yQnF_4iow/edit?usp=sharing)! Included in the document is our project roadmap/proposed timeline.

## e) Project Roadmap
Check it out in our detailed solution report above!

## f) Getting Started

### Log in to your IBM Cloud account
You'll need an IBM Cloud account to create Cloudant Databases, start up the IBM Watson Internet of Things Service, as well as launch a Node-RED app.

### Creating a database to store Chute addresses
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

### Create a Node-RED application connected to an IoT Service
This is where we'll import our Node-RED flow from the .json file included in our repo. 

#### Step 1: Start a Node-RED application
On your IBM Cloud Platform, click on catalog, and then search for the 'Node-RED App' tile. Name it "Node RED TroubleChute app", and under Service Details - Pricing Plan, select the existing Cloudant Instance, "Cloudant-TroubleChute". Ensure tat the region should be Dallas. Click Create. 

#### Step 2: Create an Internet of Things Platform Service
Click on 'Catalog', then search for the Internet of Things Platform tile. Once there, ensure the region is Dallas, then name the service "Internet of Things Platform-TroubleChute". Then, press create. Then, press 'Launch' to open a window with the IBM Watson IoT Platform, which we will navigate back to later.

#### Step 3: Connect IoT Service to app
Go to Resource List -> Apps -> Node RED TroubleChute app. Then, press 'connect existing services' to connect to the "Internet of Things Platform-TroubleChute" service

#### Step 3: Deploy your Node-RED app.
Head back to Resource List -> Apps -> Node RED TroubleChute app. Then, click on "Deploy your app". Press 'New' to create a new IBM Cloud API key. Leave the memory allocation as 128mb if on lite account, ensure region is Dallas. Click next, then ensure the DevOps toolchain name is "NodeREDTroubleChuteapp", with region as Dallas, and proceed. After a few moments, the status should be In Progress. Wait for the deploy stage to finish running.

#### Step 4: Update dependences on the package.json file



### Start a simulated device on IBM Watson IoT Platform
We will create a device to simulate readings being sent from the temperature and smoke sensos at a chute. 

#### Step 1: Create simulated device
Return to the IBM Watson IoT Platform. Create a simulated device by going through the tutorial [here](https://cloud.ibm.com/docs/IoT?topic=IoT-sim_device_data#sim_device_data). Device type should be "TempSensor"



```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
