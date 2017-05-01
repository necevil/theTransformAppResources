InfusionSoft Custom Fields
=====
Custom Fields in InfusionSoft are used to store information that can be used (manually or programatically) to tell more about a User, or to customize the Email text that a user will receive.

List of Available Custom Fields
-----
A list of availabel custom fields can be retrieved from the current InfusionSoft REST Api here: https://developer.infusionsoft.com/docs/rest/#!/Contact/listCustomFieldsUsingGET

Since not all developers will have easy access to the REST Api we also keep a secondary list of Available Custom Fields in Git, here: 

>https://github.com/necevil/theTransformAppResources/blob/master/CustomFields/InfusionSoft-CustomFields.json

A Custom Fields Example
----
We use the Custom field "Transformation Selected" (which has a Custom Field Id of: 20) to indicate the name of the Transformation the the user hasen chosen to Pursue.

Let's assume the User selects "Weight Loss" for their Transformation, and our App then UPDATES their InfusionSoft Contact so that the "Transformation Selected" Custom Field has a value of "Weight Loss"

We can use that information, and InfusionSoft's internal Logic, to send the user a Customized campaign of Emails that specifically targets the App functionality that would be useful to App Users who are looking to lose weight, while ommiting those features designed for App Users who are more inshape.

**Note:** *In the above Example we would also Apply the "App On-Boarding: Transformation Selected" Tag to Initiate the User into the Automated Email Campaign that would analyze the "Transformation Selected" Custom Field in order to send teh appropriate emails to the user.*

Improved Conversion Rating / User Rention
----
Custom Fields allow us to customize our message to each user's experience specifically.  This (over time) will result in a substantially higher percentage of total Users converting from Free Trial Users to Paid Subscribers.

Custom Fields Strategy in a Sentance
====
Where Applying "Tags" provides a method to Start / Initiate a Campaign for a Specific User, Custom Fields provide the information that we use to customize the Campaign Message to be sure it is relevent to each user.

Working with Custom Fields (Legacy XMLrpc API)
=====
We use the following NPM Pacakge / Library in our Ionic App to work with the InfusionSoft API:

>https://www.npmjs.com/package/infusionsoft

While InfusionSoft recommends using the REST Api, doing so would require writing some light functionality to create individual POST / GET responses using Axios (or something similar).

The methods we need to use to work with Custom Fields in the above NPM package are not well documented.  We would like to update this section with information from Sanjeev and Summit at some point in the future.

It is likely that the user data will be formatted similarly to teh REST Api found / tested below.



Working with Custom Fields (REST API)
=====
As mentioned previously, you can get a list of Custom Fields from the REST Api endpoint located here: 

>https://developer.infusionsoft.com/docs/rest/#!/Contact/listCustomFieldsUsingGET

To actually add a Custom Field value to a user, you need to do one of: CREATE (https://developer.infusionsoft.com/docs/rest/#!/Contact/createContactUsingPOST), UPDATE (https://developer.infusionsoft.com/docs/rest/#!/Contact/updateContactUsingPATCH) or alternately UPDATE or CREATE (https://developer.infusionsoft.com/docs/rest/#!/Contact/createOrUpdateContactUsingPUT)

1. Custom Fields are supplied as an array property when you do an Insert / Upsert that takes ONLY the Custom Field Id and supplies the value you want to assign to that custom field as a part of the "content" property.
2. In order to get the Custom Field Id you need to use the following REST Api endpoint which will return all of your Custom Fields and their Id which you need to supply to assign a value.
3. Once you have the list of Custom Fields and their Id, find the Id for the field you want to update (lets say it's 36)
4. Now you can format your JSON to pass to Create or Update.  It should look something like this:

```
{
  "duplicate_option": "Email",
  "email_addresses": [
    {
      "email": "somemail@gmail.com",
      "field": "EMAIL1"
    }
  ],
  "phone_numbers": [
    {
      "extension": "",
      "field": "PHONE1",
      "number": "6666666666",
     "type": "Work"
    }
  ],
  "custom_fields": [
    {
      "id": 36,
      "content": "126lbs"
    },
    {
      "id": 37,
      "content": "5 feet 9 inches"
    }
  ]
}
```

5. The custom_fields array object never references the field's name (In this case that would be Weight).  EVERY entry in this array must have both an id property and a content property in order to be valid (see the similarities between 36 and 37).   You will specify the id and the value (which is held in the content property) for each.  
6. Submit the above JSON to the following REST Api endpoint and you will Update OR Create the contact for the above user: https://developer.infusionsoft.com/docs/rest/#!/Contact/createOrUpdateContactUsingPUT


***Notes:*** If you have any problems you should add the custom fields manually from the InfusionSoft admin area and then use the following REST API endpoint to pull down the contact you just added the custom property to: https://developer.infusionsoft.com/docs/rest/#!/Contact/loadUsingGET This will allow you to better visualize how the Create / Upsert API's expect to receive the data.

DONT FORGET: You need to supply the optional_properties Parameter to TELL the above GET endpoint to return custom_fields.  Othewise the Contact instance that is returned will leave off the custom_fields which is probably all you are having trouble with anyway.