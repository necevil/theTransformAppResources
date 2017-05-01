# theTransformAppResources
This repository is created as an intermediary source of information that bridges between the InfusionSoft API Documentation (found here: https://developer.infusionsoft.com/docs/rest/#!/Contact/applyTagsToContactIdUsingPOST), our chosen / utilized Nodejs InfusionSoft Library (found here: https://www.npmjs.com/package/infusionsoft) and our overall TransformApp codebase.

The goal of this resource is to provide quick access to the tools and decisions we have made in regards to connecting with, updating, and managing the interface between our Ionic App, our DynamoDB backen and our InfusionSoft account (found here: https://ti328.infusionsoft.com/app/nav/link?navSystem=nav.mynav&navModule=nav.home.dashboard)

Crossing over to InfusionSoft
=====
There are two main ways our Development team can interface with the Marketing / CRM InfusionSoft system.

1. Tags
2. Custom Fields

About Tags
====
InfusionSoft "Tags" (Previously referred to as "Groups" by the Legacy XMLrpc API) are used to trigger Automation, which allows us to start or stop Campaigns for a Given user.

See our Tags Documentation: 
https://github.com/necevil/theTransformAppResources/tree/master/Tags-Groups

Tags should be applied in Real Time AS OFTEN AS POSSIBLE.

This is beacuse the email marketing that is triggered by our Tags follows the user very closely (particularly during On-Boarding).  

If we do not apply Tags in Real Time, or close to Real Time as possible, users may frequently receive emails that are not relevent to them, which will Usually result in them Blocking future emails, or worse yet, reporting a Complaint to Gmail or their ISP (which results in fewer of our emails being seen by ALL users)

Custom Fields
======
Custom Fields DO NOT trigger changes in a User's Campaign and only secondarily influence which emails are received.

Custom Fields can be updated either in Real Time, or at a later time, via a script.  Real Time is better when possible, but in some cases we may want to conserve resources for Custom Fields that are not as relevent.

One Custom Field that should be updated in Real Time:
------
Transformation Selected

Some Custom Field that can be Updated by Script:
------
User Birthday
User Address

Available Custom Fields can be found here: {coming soon}

More Information on Tags vs Custom Fields
====
InfusionSoft provides a good article that describes when to use Tags vs when to use Custom Fields, you can read that here: 

>http://help.infusionsoft.com/userguides/get-started/create-custom-fields/custom-fields-vs-tags

