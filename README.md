# theTransformAppResources
This repository is created as an intermediary source of information that bridges between the InfusionSoft API Documentation (found here: https://developer.infusionsoft.com/docs/rest/#!/Contact/applyTagsToContactIdUsingPOST), our chosen / utilized Nodejs InfusionSoft Library (found here: https://www.npmjs.com/package/infusionsoft) and our overall TransformApp codebase.

The goal of this resource is to provide quick access to the tools and decisions we have made in regards to connecting with, updating, and managing the interface between our Ionic App, our DynamoDB backen and our InfusionSoft account (found here: https://ti328.infusionsoft.com/app/nav/link?navSystem=nav.mynav&navModule=nav.home.dashboard)

Crossing over to InfusionSoft
=====
There are two main ways our Development team can interface with the Marketing / CRM InfusionSoft system.

1. Tags
2. Custom Fields

UPDATE or CREATE with API (First Contact with InfusionSoft)
====
As soon as an App User enter's their Email Address during account creation / On-Boarding, we need to Apply our first Tag.  

Before we can Appy a Tag we need to make sure the User's InfusionSoft Contact Exists (retrieve the Contact Id) and then UPDATE the user's Install DATE (Must be the first day of Trial) as all other InfusionSoft functions rely on this information.

**App Download Date** (Custom Field id: 18)
Is a Custom Field and must be a DATE type object that represents the Day the User installed the App and Began their Free Trial.  

To check to see if a User exists in InfusionSoft based on Email they entered during Account Creation (should be the same address their Verification email is sent to).  Use the following Method:
>ContactService.findByEmail: client.findByEmail(email, fMap, callback)

If the user exists the response will contain the User's Contact Id.  If the user exists we can then use the following to update the Contact.
>ContactService.update: client.updateCon(contactId, contactData, callback)

If the User is not found by Email in InfusionSoft we should use the following to create the user (with Duplicate check... JUST IN CASE):
>ContactService.addWithDupCheck: client.addWithDupCheck(data, dupCheckType, callback)

Critical Information to add to InfusionSoft Contacts
=====
In addition to **App Download Date** (Custom Field) we also need to add the following to the InfusionSoft Contact:

EMAIL
First Name
Last Name
Address (If Available)
Phone (If Available)

The most important information is the First Name, Last Name and **App Download Date**


Custom Fields
======
Full Docs on Custom Fields can be found here:
https://github.com/necevil/theTransformAppResources/tree/master/CustomFields

Available Custom Fields that we want to Use for App Tracking can be found here:
https://github.com/necevil/theTransformAppResources/blob/master/CustomFields/InfusionSoft-CustomFields.json

The MOST Important Custom Field is:
**App Download Date** (Custom Field id: 18)

Equally important, we need to make sure ALL Users have their Email added to the Contact when we create it via the API.

In general an App User should be updated with Available Custom Fields as soon as that Information becomes available to the Back-end System /DynamoDB.  In otherwords, if we are storing information about a user in DyanmoDB, there is a rasonable chance we should also be Updating InfusionSoft Custom Fields and Adding InfusionSoft Tags.

**About Custom Fields**
Custom Fields DO NOT trigger changes in a User's Campaign and only secondarily influence which emails are received.

Custom Fields can be updated either in Real Time, or at a later time, via a script.  Real Time is better when possible, but in some cases we may want to conserve resources for Custom Fields that are not as relevent.

One Custom Field that should be updated in Real Time:
------
Transformation Selected

Some Custom Field that can be Updated by Script:
------
User Birthday
User Address

Available Custom Fields can be found here: https://github.com/necevil/theTransformAppResources/blob/master/CustomFields/InfusionSoft-CustomFields.json

About Tags
====
InfusionSoft "Tags" (Previously referred to as "Groups" by the Legacy XMLrpc API) are used to trigger Automation, which allows us to start or stop Campaigns for a Given user.

See our Tags Documentation: 
https://github.com/necevil/theTransformAppResources/tree/master/Tags-Groups

Tags should be applied in Real Time AS OFTEN AS POSSIBLE.

This is beacuse the email marketing that is triggered by our Tags follows the user very closely (particularly during On-Boarding).  

If we do not apply Tags in Real Time, or close to Real Time as possible, users may frequently receive emails that are not relevent to them, which will Usually result in them Blocking future emails, or worse yet, reporting a Complaint to Gmail or their ISP (which results in fewer of our emails being seen by ALL users).

List of Available / Active Tags: 

>https://github.com/necevil/theTransformAppResources/blob/master/Tags-Groups/AvailableTags.md

More Information on Tags vs Custom Fields
====
InfusionSoft provides a good article that describes when to use Tags vs when to use Custom Fields, you can read that here: 

>http://help.infusionsoft.com/userguides/get-started/create-custom-fields/custom-fields-vs-tags

