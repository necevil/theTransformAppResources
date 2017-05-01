
InfusionSoft "Tags" (known as "Groups in Legacy API)
====
For InfusionSoft, the most important aspect of successful use is triggering "Tags" to be applied to users when they achieve specific milestones / events.  

This is because "Tags" applied to a contact / user trigger InfusionSoft to send Automated emails that have been designed in advance.  While Custom Fields are also important, adding "Tags" to a user quickly (hopefully in real time) upon a given Event is critical.

It will not be sufficient to update InfusionSoft via a backend script which runs every few hours since emails are often very time sensitive.

"Tags" in InfusionSoft are somewhat similar to "Events" for Google Analytics and most of the time need to be applied at similar times.  Tags are updated via the InfusionSoft user Tagging API Endpoint found here: https://developer.infusionsoft.com/docs/rest/#!/Contact/applyTagsToContactIdUsingPOST

For this reason it makes sense to apply Tags from written functions / methods that will also trigger Events or Conversions in Google Analytics.  Only with both Google Analytics Event data and InfusionSoft Tags can we maximise our conversion rating and ensure the highest possible chance of success for the team.

For the NPM InfusionSoft package (found here: https://www.npmjs.com/package/infusionsoft) "Tags" are described using the Legacy "Group" methods:

>***ContactService.addToGroup***: client.grpAssign(contactId, groupId, callback)

>***ContactService.removeFromGroup***: client.grpRemove(contactId, groupId, callback)

***In order to apply a Tag***
You must have two critical pieces of information:
1. The User's InfusionSoft Contact Id.
2. The Key / Tag Id for the corresponding Tag (called a Group by the Legacy API) that you wish to apply.

Finding a User's Contact Id / Adding a Contact
======
In order to Tag a user we must first have their Contact Id number.  Since we *MAY* or *MAY NOT* be creating a new user in InfusionSoft we need to use the following UPDATE or CREATE REST Api methodolgy: 

For Legacy this corresponds to the following:

>***ContactService.addWithDupCheck***: client.addWithDupCheck(data, dupCheckType, callback)

We need to make sure we check for Duplicates since the User may already have a Contact / Contact Id if they signed up for the Mailing List previously.

Another useful Legacy API method would be:

>***ContactService.findByEmail***: client.findByEmail(email, fMap, callback)

With the above we can supply the current users Email and then return their information (which should include their Contact Id).

InfusionSoft API Contact Id Queries 
------
One thing that we want to avoid is having to Query InfusionSoft API often to retrieve a Contact Id number.  We can do this by performing a single Query to InfusionSoft when we do an Update or Create, or alternately by saving the Response that comes back when a user is created into DynamoDB.

Either way, we need to save the InfusionSoft Contact Id for EVERY app user to that user's DyanmoDB model instance.  We also need to have this available locally on the device so that we can quickly execute addToGroup method calls without needing to first send another query to InfusionSoft API to retrieve the Contact Id (not necessary since we will have it saved).


Finding the Appropriate Tag Id
=====
Once of the Key Reasons we created this Documentation Repository was so that we had an easy place to access a more detailed list of Tag Id's and descriptions than would be available in InfusionSoft by Default.

See list of Available Tags: 

>https://github.com/necevil/theTransformAppResources/blob/master/Tags-Groups/AvailableTags.md

As of this writing there is no API method available to retrieve a list of Tag Id's (Legacy API refers to "Tags" as "Groups").

For this reason we have created a separate Markdown formatted list of Available Tags which includes detailed descriptions of their intended use in addition to descriptions of WHEN that Tag should be applied.

See list of Available Tags: 

https://github.com/necevil/theTransformAppResources/blob/master/Tags-Groups/AvailableTags.md

The above list will contain entries that look something like this:
***Opened App*** ID=178
Users who have opened the app but have not yet verified their email or completed their registration.

***Verified Email / Created Account*** ID=180
Users who have verified their Email but have yet to select / choose a transformation. User should be tagged the first time they open the app after Verifying their email.

In the above examples we describe the User's and WHEN that user would no longer be a part of the Tag.  It is up to the Developers to appropriately determine the time at which the Tag is applied.

Example Instance
---- 
When looking at the "Opened App" Tag:
Since "Opened App" only includes users UNTIL they have verified their Email address, a good time to apply this Tag would be when the Verification Email is being sent.

We would then Apply the "Verified Email" tag as soon as the user successfully verifies their email address.

*Note: The Name of the Tag is for Human use only, InfusionSoft does not require it's use to Add or Remove a Tag in either the REST Api or the Legacy Api as ONLY the Tag's Id is referenced.*


Testing our Applied Tags
====
In order to make testing possible, we have created a Testing Gmail account (which will receive any emails when that User has been Successfully Tagged).  We have attached that Testing Gmail account to an InfusionSoft user.

Therefore we can use these Testing Accounts to accurately test not only De-Duplication (since the user already exists in InfusionSoft) but also whether or not Tags have been successfully Applied.

Test User
--------
***Gmail Account***
U: thetransformappwelcome@gmail.com
P: theTransformApp2017

InfusionSoft Contact ID: 194726
First Name: theTransformApp
Last Name: WelcomeTest


You may use the API to Query based on the Contact Id to find out whether your code is applying a given tag correctly, in this way you may test your code to be sure Tags are applied.

You can also use the REST Api to check for a User's Contact / Tags if you do not want to write custom code to do that using the NPM Package / Legacy API: 

The REST API Documentation site allows a Developer to login (via the Authorize link in the Top Left) to actually test REST API requests FROM the documentation site (requiring no custom code to be written): 

>https://developer.infusionsoft.com/docs/rest/#!/Contact/listAppliedTagsUsingGET

The REST API endpoint mentioned above will return all Tags applied to a user which is useful in testing to see if your code correctly applied a Tag to an InfusionSoft Contact as expected.

Real Time Contact Tagging
========
We need to be sure not only that the Tags are being applied, but that they are being applied in Real Time.

This is because on my side I will be setting up InfusionSoft to REMOVE old tags, as the user moves through our process.  If the Tags are not applied in Real Time we will be sending emails that DO NOT correspond to a Users experience and in this way our conversion rating (going from Free to Paid) will drop dramatically compared to what should be possible.

You do not necessarily need to log in to Gmail using that user, but we wanted to include this so that you have positive feedback that the whole system is functioning.   

Currently Working
======
Right now the ONLY Tag that will trigger an email to be sent is the "Opened App". Hopefully by the time you get to it I will also have a campaign working for "Verified Email" which is ID: 180.

Opened App ID=178
Users who have opened the app but have not yet verified their email or completed their registration.

Verified Email / Created Account ID=180
Users who have verified their Email but have yet to select / choose a transformation. User should be tagged the first time they open the app after Verifying their email.
