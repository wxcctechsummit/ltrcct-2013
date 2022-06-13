---
title: 'Lab 3: Live Chat Configuration'
---


# Table of Contents
- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
    - [Lab Objective](#lab-objective)
    - [Pre-requisite](#pre-requisite)
    - [Quick Links](#quick-links)
- [Lab Section](#lab-section)
  - [Step 1. Chat Asset creation & register to Webex CC](#step-1-chat-asset-creation--register-to-webex-cc)
  - [Step 2. Chat Template creation for website integration](#step-2-chat-template-creation-for-website-integration)
  - [Step 3. Chat Entry Point and Queue creation](#step-3-chat-entry-point-and-queue-creation)
    - [1. Create Entry Point in Management Portal](#1-create-entry-point-in-management-portal)
    - [2. Create Queue in Management Portal](#2-create-queue-in-management-portal)
    - [3. Skill and Skill profile](#3-skill-and-skill-profile)
  - [Step 4. Website Settings](#step-4-website-settings)
    - [1. Configure Live Chat widget](#1-configure-live-chat-widget)
    - [2. Verify that live chat widget loads](#2-verify-that-live-chat-widget-loads)
  - [Step 5. Create/Upload Live Chat flow](#step-5-createupload-live-chat-flow)
    - [1. Initial flow loading](#1-initial-flow-loading)
    - [2. Start node and Custom Variables](#2-start-node-and-custom-variables)
    - [3. Select Live Chat form](#3-select-live-chat-form)
    - [4. Edit Queue Task node](#4-edit-queue-task-node)
  - [Step 6. Verification - start live chat and accept the request](#step-6-verification---start-live-chat-and-accept-the-request)
  - [Step 7. Optional -  Enhance flow](#step-7-optional----enhance-flow)
    - [1. Add Branch to handle Dropdown form field](#1-add-branch-to-handle-dropdown-form-field)
  - [Back to top](#back-to-top)
    - [Congratulations, you have completed this section!](#congratulations-you-have-completed-this-section)


# Introduction

### Lab Objective

In this Lab, we will go through the tasks that are required to complete the basic Live hat integration. You will be able to initiate a Chat contact to the Contact Center from a sample website and be able to accept/respond to the contact by logging in as an agent.  

In this lab you you will be configuring Service, Chat Assets, Entry Point, Queue, Chat Template, Website Settings, and corresponding workflows.


### Pre-requisite

1. You received an admin credentials to configure in Management Portal and Webex Connect.
2. You have successfully completed the previous Lab **Preconfiguration**

### Quick Links

> Control Hub: **[https://admin.webex.com](https://admin.webex.com){:target="_blank"}**\
> Portal: **[https://portal.wxcc-us1.cisco.com/portal](https://portal.wxcc-us1.cisco.com/portal){:target="_blank"}**\
> Agent Desktop: **[https://desktop.wxcc-us1.cisco.com](https://desktop.wxcc-us1.cisco.com){:target="_blank"}**\
> Workflows: **[GitHub page](https://github.com/CiscoDevNet/webexcc-digital-channels){:target="_blank"}**\
> Connect: https://cl1pod**\<ID\>**.imiconnect.io/ (where **\<ID\>** is your POD number)

# Lab Section

## Step 1. Chat Asset creation & register to Webex CC

- Login to your respective Webex Connect UI using the provided URL https://cl1pod**X**.imiconnect.io/ (where **X** is your POD number).

- Navigate to `Assets` > `Apps` > `Configure New App` > `Mobile / Web`
<img align="middle" src="images/Lab3_1.gif" width="1000" />

- Provide a `Name`

- Toggle/Enable `Live Chat / In-AppMessaging` to "ON" and choose `PRIMARY TRANSPORT PROTOCOL` as  MQTT" & `SECONDARY TRANSPORT PROTOCOL` as Web Socket" and enable `Use Secured Port` and `SAVE`.

<img align="middle" src="images/Lab3_2.jpg" width="700" />

- Select `REGISTER TO WEBEX CC` and choose the Service you have created and REGISTER

<img align="middle" src="images/Lab3_3.jpg" width="1000" />

- In the resulting window, select a service under which this asset would be managed
<img align="middle" src="images/Lab3_4.jpg" width="600" />

- Verify that the `Register to Webex CC` option is now disabled and there is a message indicating the time when the asset was registered along with the service to which it is assigned. 
<img align="middle" src="images/Lab3_5.jpg" width="1000" />

- Click the back arrow next to go back to the list of Apps. Then take note of the Application ID (App ID). We will need this later so please copy this ID somewhere handy like a text file or take note of it.
  
<img align="middle" src="images/Lab3_33.jpg" width="400" />

[To top of this lab](#table-of-contents)

## Step 2. Chat Template creation for website integration

- Chat template creation allows you to configure a pre-defined chat form that will presented to the customer. Data points can be collected from the customer in a chat-like interface.

- From Webex Connect interface, go to `TOOLS` > `Templates` then click on `Add New Template`

<img align="middle" src="images/Lab3_8.jpg" width="200" />

- Provide a Name and choose Channel as `Live Chat / In-APP Messaging`

- Message Type as `Form`

- Provide the Title as `Welcome to Webex CC Live Chat` or some other message.
  
<img align="middle" src="images/Lab3_9.jpg" width="1000" />

- We will be adding form fields now. Firstly the Name. Click on `Add field` and then fill in the details as per the screenshot

<img align="middle" src="images/Lab3_10.jpg" width="400" />

-Continue by adding the `Email` and `Reason` fields in the same manner with the info in this table.

|Type|Name|Label|Mandatory Field|
|-|-|-|-|
|Name|Name|Name|true|
|Email|Email|Email|true|
|Dropdown|Reason|Reason for Contacting Us|true|
|Text|Description|Description|false|

Here is a screenshot of the Dropdown configuration with 2 options, one for Sales and one for Support. We will use this later to perform Skills Based Routing so chats are routed to most skilled agents.

<img align="middle" src="images/Lab3_11.jpg" width="600" />

- Finally click `SAVE`

<img align="middle" src="images/Lab3_12.jpg" width="1000" />

[To top of this lab](#table-of-contents)

## Step 3. Chat Entry Point and Queue creation

### 1. Create Entry Point in Management Portal 

- Click on **_Provisioning_** and select **_Entry Points/Queues_** > **_Entry Point_**.

- Click on `New Entry Point`.

- Input **_Name_** as `Chat_EP`.

- Select `Chat` in the **_Channel Type_** section.

- Leave the **_Asset Name_** as the configured value earlier.

- Set **_Service Level Threshold_** as `300` seconds.

- The **_Time Zone_** can stay as default value.

- Click on **Save** after comparing your values with the screenshot below.

<img align="middle" src="images/Lab3_6.jpg" width="800" />

### 2. Create Queue in Management Portal 

- Click on **_Provisioning_** and select **_Entry Points/Queues_** > **_Queue_**.

- Click on `New Queue`.

- Input **_Name_** as `Chat_Q_SBR`.

- Select `Chat` in the **_Channel Type_** section.

- Select **_Queue Routing Type_** as `Skills Based`.

- Set **_Agent Selection_** as `Best Available Agent`

- In the the **_Chat Distribution_** click on **Add Group** and select `Team1`.

- Set **_Service Level Threshold_** as `90` seconds.

- Set **_Maximum Time in Queue_** as `600` seconds.

- The **_Time Zone_** can stay as default value.

- Click on **Save** after comparing your values with the screenshot below.

<img align="middle" src="images/Lab3_7.jpg" width="800" />

### 3. Skill and Skill profile

- We are now going to configure skills and skill profiles for the skills based Queue Click on **_Provisioning_** and select **_Skills_** > **_Skill Definition_**.

- Click on `New Skill Definition`

- Set **_Name_** as `Sales`

- Set **_Service Level Threshold_** as `60` seconds.

- Set **_Type_** as Boolean.

- Click on **Save**

<img align="middle" src="images/Lab3_27.jpg" width="600" />

- Create another Skill definition of same type `Boolean` with **_Name_** as `Support`

<img align="middle" src="images/Lab3_28.jpg" width="600" />

- Next we'll create 2 Skill profiles, one for Sales and one for Support. Click on **_Provisioning_** and select **_Skills_** > **_Skill Profile_**.

- Click on `New Skill Profile`

- Set **_Name_** as `Sales`

- Select the `Sales` Skill and set the **_Skill Value_** to `True`

- Select the `Support` Skill and set the **_Skill Value_** to `False`

- Click on **Save**

<img align="middle" src="images/Lab3_29.jpg" width="600" />

- Create another Skill Profile with Name `Support` with Sales skill value to False and Support skill value to True
  
<img align="middle" src="images/Lab3_30.jpg" width="600" />

- Finally we'll assign the Sales Skill profile to our agent. Click on **_Provisioning_** and select **_Users_**

- Edit the Agent user created earlier by clicking on the 3 dotted menu

<img align="middle" src="images/Lab3_31.jpg" width="600" />

- In the agent settigns, Select `Skill Profile` as `Sales` and click **Save**

<img align="middle" src="images/Lab3_31.jpg" width="600" />

[To top of this lab](#table-of-contents)

## Step 4. Website Settings

### 1. Configure Live Chat widget
- From Management Portal, access the menu and cross launch **New Digital Channels Admin Portal**  by choosing `New Digital Channels`
<img align="middle" src="images/Lab3_13.jpg" width="400" />

- Goto `Assets` > search and edit the chat asset which we created earlier in **Step 1**

<img align="middle" src="images/Lab3_14.jpg" width="400" />

- Scroll down and choose `Save Changes`

<img align="middle" src="images/Lab3_15.jpg" width="200" />

- Scroll to top of the page and choose `Websites` and then click `Add Website`

<img align="middle" src="images/Lab3_16.jpg" width="400" />

- Enter the respective fields as per Screenshots below. Note we are going to insert the chat bubble into an online HTML editor for testing www.w3schools.com. The `Domain` field should contain the domain where you will insert the chat bubble.

<img align="middle" src="images/Lab3_17.jpg" width="1000" />

- Take a moment to explore the rest of the customization options for the chat bubble and then click `SAVE CHANGES`

- Scroll up, select the `Appearance` tab and change the settings (Widget Color, Logo, Emojis, attachments etc.;) as per your requirement and `SAVE CHANGES`

- Select the `Widget Visibility` tab and click on `Show without restriction` and `SAVE CHANGES`

<img align="middle" src="images/Lab3_26.jpg" width="1000" />

- Explore and `Banned Customers` tab so you are familiar with the settings but we are not going to change the default settings for those at this point

### 2. Verify that live chat widget loads

- There's still a few bits to configure but we can now verify that the live chat widget loads.
- Go-back to edit the channel livechat asset, select Installation tab and Copy the chat script code.
<img align="middle" src="images/Lab3_18.jpg" width="1000" />

- Open a new tab in your browser and navigate to [W3Schools Online HTML Editor](https://www.w3schools.com/tryit/tryit.asp?filename=tryhtml_hello){:target="_blank"}.
 
- Paste de chat bubble code just above the `</body>` tag

<img align="middle" src="images/Lab3_24.jpg" width="1000" />

- Click the Run button and the chat bubble should appear on the right side of the HTML online editor. Verify your settings if that does not happen and contact the lab proctor.

<img align="middle" src="images/Lab3_25.jpg" width="1000" />

- Click on the chat bubble icon and it should show the previously configured livechat widget. 

[To top of this lab](#table-of-contents)

## Step 5. Create/Upload Live Chat flow

### 1. Initial flow loading
- Download the default inbound chat flow from the [GitHub page](https://github.com/CiscoDevNet/webexcc-digital-channels){:target="_blank"}.

- Navigate to **Webex Connect Flows** -> **v2.1** -> **Live Chat Inbound Flow.workflow.zip**, select the zip file and click download.

- Unzip the downloaded file.

- Go to Webex Connect, click on **Services** and select the service in which the Asset is created in step 2. It should be **My First Service**

- In the service click on **FLOWS** -> **CREATE FLOW** .

<img align="middle" src="images/Lab3_19.jpg" width="600" />

- Enter the **FLOW NAME** as **Live Chat Inbound Flow**, select the **TYPE** as **Work Flow** and under **METHOD** select **Upload a flow**.

- Drag and drop the **Live Chat Inbound Flow.workflow** flow file that you unzipped, click **CREATE** and then click **SAVE**.

<img align="middle" src="images/Lab3_20.jpg" width="1000" />

### 2. Start node and Custom Variables

- A page will load with the imported workflow. We must make some changes to the default inbound flow based on our setup.

- First Click `Save` in the `Configure APP Event` page that loaded, this defines what will trigger the flow and the default settings are already good.
  
<img align="middle" src="images/Lab3_21.jpg" width="1000" />

- Click on the gear button on the top right to load the flow settings dialog

<img align="middle" src="images/Lab3_34.jpg" width="600" />

- Select the Custom Variables tab and set the following variable defaults:

*appid*: Set it to the value you copied in Step1

*domain*: Set it to `www.w3schools.com`

*liveChatDomain*: Set it to `www.w3schools.com`

<img align="middle" src="images/Lab3_35.jpg" width="1000" />

- In your production setup domain should be set to your website's domain

### 3. Select Live Chat form

- We must select the right Live Chat Template as configured earlier so that the right Form is presented to the customer. Click on the `Pre-chat form` node and select `Form Template` as configured earlier and `Save`

<img align="middle" src="images/Lab3_22.gif" width="400" />

3. The same must be done in the Receive node, double click on it and select the Form from the dropdown menu and `Save`

<img align="middle" src="images/Lab3_23.gif" width="1000" />

### 4. Edit Queue Task node

- In the created workflow find the **Queue Task**, click twice, select the **QUEUE NAME** as **Chat_Q_SBR** and add Skill requirement for Sales to be True and click on **SAVE**.

<img align="middle" src="images/Lab3_36.jpg" width="1000" />

- Finally click on Make Live on top right corner -> Select the Application/Asset that we have created and click `Make Live`.

<img align="middle" src="images/Lab3_37.jpg" width="1000" />

- Wait for 2 minutes and verify that the flow is published successfully.

 <img align="middle" src="images/Lab3_38.jpg" width="1000" />

[To top of this lab](#table-of-contents)

## Step 6. Verification - start live chat and accept the request

- Open a new tab and login to the Agent Desktop and make the agent Available (if you haven't done already in Lab2).

<img align="middle" src="images/Lab2_Agent1.png" width="1000" />

- Go back to the tab where you opened [W3Schools Online HTML Editor](https://www.w3schools.com/tryit/tryit.asp?filename=tryhtml_hello){:target="_blank"} and pasted the live chat widget code. 

- Click `Start Conversation`

<img align="middle" src="images/Lab3_39.jpg" width="400" />

- Fill in the form with customer options

<img align="middle" src="images/Lab3_40.jpg" width="1000" />

- The Live Chat will be offered to the agent. Click "Accept" to handle the SMS.

<img align="middle" src="images/Lab3_41.jpg" width="1000" />

- The form submission will be presented to the customer

<img align="middle" src="images/Lab3_42.jpg" width="1000" />

- Type a response and hit send button.

<img align="middle" src="images/Lab3_43.jpg" width="400" />
<img align="middle" src="images/Lab3_44.jpg" width="1000" />

- End the contact

<img align="middle" src="images/Lab3_45.jpg" width="700" />

- Add wrap up and close the task. 

<img align="middle" src="images/Lab3_46.jpg" width="400" />

## Step 7. Challenge Lab - Enhance flow
 
### 1. Add Branch to handle Dropdown form field

- Add a Branch node before the Queue Task node that differentiates between Sales and Support from the form's dropdown menu and queue's with a different Skill requirement

[Back to top](#table-of-contents)
---

### Congratulations, you have completed this section! 

<script>
function mainPage() {window.location.href = "https://wxcctechsummit.github.io/wxcclabguides/LTRCCT-2013/Home.html";}
function nextLab() 
 {
 window.location.href = "https://wxcctechsummit.github.io/wxcclabguides/LTRCCT-2013/Lab4_FBM.html";
 }
</script>

<div id="button-row">
<button onclick="mainPage()" style="
  border-radius: 5px;
  background-color: rgb(116,191,75);
  padding: 10px;">Home Page</button>

<button onclick="nextLab()" style="
  position: absolute;
  right: 200px;
  border-radius: 5px;
  background-color: rgb(116,191,75);
  padding: 10px;">Go to the Next Lab</button>

</div>
