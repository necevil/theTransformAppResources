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

Let's assume the User selects "Weight Loss" for their Transformation.

We can use that information to send the user a Customized campaign of Emails that specifically targets the App functionality that would be useful to App Users who are looking to lose weight, while ommiting those features designed for App Users who are more inshape.

**Note:** *In the above Example we would also Apply the "App On-Boarding: Transformation Selected" Tag to Initiate the User into the Automated Email Campaign that would analyze the "Transformation Selected" Custom Field in order to send teh appropriate emails to the user.*

Where Tags provide the methodology used to Initiate the Campaign, Custom Fields provide the information that we use to customize the Campaign Message.