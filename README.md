
# Anypoint Template: Salesforce to Siebel Account Migration

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
	* [Siebel Considerations](#siebelconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
I want to synchronize accounts from Salesforce instance to Oracle Siebel Business Objects.

This template should serve as a foundation for the process of migrating accounts from Salesforce instance to Oracle Siebel Business Objects, being able to specify filtering criteria and desired behavior when an account already exists in the Siebel instance. 

As implemented, this template leverage the [Batch Module](http://www.mulesoft.org/documentation/display/current/Batch+Processing).
The batch job is divided to Process and On Complete stages.
The template will query SalesForce for all the existing Accounts that match the filter criteria.
During the Process stage the template will create the accounts in Siebel and update those which already exist.
Finally during the On Complete stage the template will output statistics data into the console and send a notification email with the results of the batch execution to the group of configured email addresses.

# Considerations <a name="considerations"/>

To make this Anypoint template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made in order for all to run smoothly. **Failing to do so could lead to unexpected behavior of the template.**



## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce for this template to work.

To have this template work as expected, you should be aware of your own Salesforce field configuration.

### FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US

### As source of data

If the user configured in the template for the source system does not have at least *read only* permissions for the fields that are fetched, then an *InvalidFieldFault* API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```


## Siebel Considerations <a name="siebelconsiderations"/>

There may be a few things that you need to know regarding Siebel for this template to work.

This Anypoint Template may be using date time/timestamp fields from the Siebel to do comparisons and take further actions.
While the template handles the time zone by sending all such fields in a neutral time zone, it cannot discover the time zone in which the Siebel instance is on.
It is up to the user of this template to provide such information. For more about Siebel time zones, see [link](http://docs.oracle.com/cd/B40099_02/books/Fundamentals/Fund_settingoptions3.html)



### As destination of data

In order to make the siebel connector work smoothly you have to provide the correct version of the siebel jars (Siebel.jar, SiebelJI_enu.jar) that works with your Siebel installation.








# Run it! <a name="runit"/>
Simple steps to get Salesforce to Siebel Account Migration running.
No matter how you run this template, this is an example of the output you'll see after hitting the HTTP endpoint:

{
    "Message": "Batch Process initiated",
    "ID": "fb324340-c0bc-11e8-9a16-2ac63fa6f77a",
    "RecordCount": 7,
    "StartExecutionOn": "2018-09-25T14:17:33Z"
}

## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Anypoint Studio from this [location](https://www.mulesoft.com/lp/dl/studio)
+ You can download Mule runtime from this [location](https://www.mulesoft.com/platform/mule)


### Importing an Anypoint Template into Studio
Anypoint Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio Project from a file system
+ Packaged Mule application (.jar)

You can find a detailed description on how to do so in this [documentation page](https://docs.mulesoft.com/).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click the Anypoint Template project folder 
+ Hover your mouse over `"Run as"`
+ Click `"Mule Application (configure)"`
+ Inside the dialog, select Environment and set the variable `"mule.env"` to the value `"dev"`
+ Click `"Run"`


### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Complete all properties in one of the property files, for example in [mule.prod.properties] (../master/src/main/resources/mule.prod.properties) and run your app with the corresponding environment variable to use it. To follow the example, this will be `mule.env=prod`. 


## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](https://docs.mulesoft.com/runtime-manager/) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.
Once your app is all set and started, supposing you choose `sfdc2siebaccountmigration` as domain name to trigger the use case, you just need to hit `http://sfdc2siebaccountmigration.cloudhub.io/migrateaccounts` and report will be sent to the email addresses configured.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Studio provides an easy way to deploy your template directly to CloudHub, for the specific steps to do so check this [link](https://docs.mulesoft.com/runtime-manager/deployment-strategies)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
To use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
+ page.size `100`
+ http.port `9090` 

#### SalesForce Connector configuration
+ sfdc.username `joan.baez@orgb`
+ sfdc.password `JoanBaez456`
+ sfdc.securityToken `ces56arl7apQs56XTddf34X`

#### Oracle Siebel Connector configuration
+ sieb.user `SADMIN`
+ sieb.password `SADMIN`
+ sieb.server `192.168.10.8`
+ sieb.serverName `SBA_82`
+ sieb.objectManager `EAIObjMgr_enu`
+ sieb.port `2321`

#### SMTP Services configuration
+ smtp.host `smtp.gmail.com`
+ smtp.port `587`
+ smtp.user `email%40example.com`
+ smtp.password `password` 

#### Mail Details
+ mail.from `batch.migrateaccounts.migration%40mulesoft.com`
+ mail.to `cesar.garcia@mulesoft.com`
+ mail.subject `Batch Job Finished Report`

# API Calls <a name="apicalls"/>
SalesForce imposes limits on the number of API Calls that can be made. Therefore calculating this amount may be an important factor to consider. This template uses just one query to Salesforce,
therefore this is not something to worry about.


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As Mule applications are based on XML files, this page is organized by describing all the XML that conform the Anypoint Template.
Of course more files can be found such as test classes and Mule application files, but to keep it simple, we focus on the XMLs.

Here is a list of the main XML files you can find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Configuration Properties](https://docs.mulesoft.com/mule4-user-guide/v/4.1/configuring-properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor, they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
This file holds the functional aspect of the Anypoint template, directed by one flow responsible of conducting the business logic.
For the purpose of this particular template the *mainFlow* just executes a [Batch Job](http://www.mulesoft.org/documentation/display/current/Batch+Processing) which handles all its logic.
This flow has Exception Strategy that basically consists on invoking the *defaultChoiseExceptionStrategy* defined in *errorHandling.xml* file.



## endpoints.xml<a name="endpointsxml"/>
This is the file where you will find the inbound and outbound sides of your integration app.
This template has only a [HTTP Connector](http://www.mulesoft.org/documentation/display/current/HTTP+Connector) as the way to trigger the use case.

### Inbound Flow
**HTTP Connector** - Start Report Generation
+ `${http.port}` is set as a property to be defined either on a property file or in CloudHub environment variables.
+ The path configured by default is `migrateaccounts` and you are free to change for the one you prefer.
+ The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub will then route requests from your application domain URL to the endpoint.



## errorHandling.xml<a name="errorhandlingxml"/>
This is the right place to handle how your integration will react depending on the different exceptions. 
This file provides [Error Handling](https://docs.mulesoft.com/mule4-user-guide/v/4.1/error-handling) that is referenced by the main flow in the business logic.



