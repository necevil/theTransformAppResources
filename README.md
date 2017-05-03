# theTransformAppResources
This repository is created as an intermediary source of information that bridges between the InfusionSoft API Documentation (found here: https://developer.infusionsoft.com/docs/rest/#!/Contact/applyTagsToContactIdUsingPOST), our chosen / utilized Nodejs InfusionSoft Library (found here: https://www.npmjs.com/package/infusionsoft) and our overall TransformApp codebase.

The goal of this resource is to provide quick access to the tools and decisions we have made in regards to connecting with, updating, and managing the interface between our Ionic App, our DynamoDB backen and our InfusionSoft account (found here: https://ti328.infusionsoft.com/app/nav/link?navSystem=nav.mynav&navModule=nav.home.dashboard)

Links to More Detailed Documentation:
1. InfusionSoft Custom Fields: https://github.com/necevil/theTransformAppResources/tree/master/InfusionSoft/CustomFields
2. InfusionSoft Tags: https://github.com/necevil/theTransformAppResources/tree/master/InfusionSoft/Tags-Groups

InfusionSoft API Quotas and Limitations
====
The most important thing to note about InfusionSoft is that they enforce several HARD Quotas, as follows.

Both Api options start with the same possible Quota HOWEVER, the REST Api Quotas CAN be increased (not yet sure of the cost) by special request and on an as needed basis per InfusionSoft customer.  The Legacy API CANNOT be increased.

Legacy Api Quotas (for XMLRPC requests)
---
Legacy API is used by the following NPM Package (which we previously believed we could use safely): https://www.npmjs.com/package/infusionsoft

Quota is as follows:
1. Maximum of 25 requests Per Second
2. Maximum of 125,000 requests Per Day
3. Maximum of 3,750,000 requests Per Month

The requests per second will be a major problem for us during CRON Script development since we have ~60,000 contacts to update or create, each one of those will need at least two API Requests to update the information via Legacy API.

60,000 x 2 = 120,000 requests (very near to per day Limit), more importantly we would have to find a way to rate-limit ourselves in terms of the 25 per-second Quota.  This would mean we need some sort of a Caching solution like Redis with Resque (see resque documentation: https://github.com/taskrabbit/node-resque)

***MOST IMPORTANT: No Increase possible for Legacy API Quota***

In order to get around this issue we may be able to use the following Limiter Package (very popular on NPM): https://www.npmjs.com/package/limiter

REST Api Quotas
----
Quota is as follows:
1. Maximum of 25 requests Per Second
2. Maximum of 125,000 requests Per Day
3. Maximum of 3,750,000 requests Per Month

Both Api options start with the same possible Quota.  REST Api Quotas CAN be increased (not yet sure of the cost) by special request and on an as needed basis per InfusionSoft customer.

This means that in all likely hood will will need to use the REST Api, and can not count on using the previously configured NPM Package as a reliable way to update our data.


Our Scripts
===
We would like to create 3 scripts to work with through the functionality we need to create base level integration with InfusionSoft.  These sulutions may be implemented with EITHER of the above APIs but as mentioned above, using the REST Api has the benefit of enabling us to add additional resources / calls per second (expanded Quotas) if we need them.

Cron Script 1 "Add Contacts"
======
This script generally applies to any User who has not yet been added to InfusionSoft.  We can tell the difference since we plan to Save the User's InfusionSoft Contact Id into their DynamoDB / Cognito user profile.  

Therefore this script will opperate on ONLY Users who DO NOT have a InfusionSoft Contact Id already set in DynamoDB.

1. API Call 1 - Query User's email to make sure they don't already exist in InfusionSoft (Users who have previously signed up for Email mailing list WILL EXIST)
2. API Call 2 - Add New User Contact to InfusionSoft (with email AND App Download Date) or Update User's Contact with Available information / custom fields if they exist.
3. Store User's InfusionSoft Contact Id with the User's Cognito identity in DynamoDB 


Cron Script 2: "Add Initial Tags"
=====
Applies to any user who does not have Tags stored in DynamoDB.  

Script loops through DyanmoDB users who do not have ANY InfusionSoft tags stored with their identities and then updates them with appropriate Tags based on the Information they have entered into the database.  This also assumes we can determine the correct Tags for a user wby refering to DynamoDB and referencing some conditions.

InfusionSoft API Calls:
1. API Call 1: Add Initial Tags based on Conditions

Cron Script 3 "Update Tags"
=====
Script to routinely update EVERY active user's Tags so they correctly reflect the user's progress.  InfusionSoft API Calls required (assuming we can read our Necessary tags from DynamoDB):
1. API Call 1: Remove Old Tags (called Groups)
2. API Call 2: Add New Tags (called Groups)

+++++++++++++++++
