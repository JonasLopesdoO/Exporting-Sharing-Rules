# Exporting Sharing Rules B2B Commerce
This repository aims to help you and your team to save time adding sharing rules.

Even if the Lightning B2B is the way to go nowadays, some projects still run in B2B for Visualforce.
This code can also help as a base knowledge for when it is necessary deploying sharing rules for other Salesforce orgs.

If you manage multiple storefronts and communities, creating sharing rules for every affected B2B Commerce custom object across every community can be a time-consuming task. To more efficiently deploy sharing rules to all of your communities, you can programmatically deploy sharing rules metadata through command-line.

This project is based on Salesforce's suggestion about adding sharing rules programmatically. 
https://help.salesforce.com/articleView?id=b2b_commerce_setup_sharing_rules.htm&type=5

All the sharing rules used in this project can be found here: https://help.salesforce.com/articleView?id=b2b_commerce_setup_sharing_rules_metadata.htm&type=5

--------------------------------------------------------------------------------------------------------------------

Introduction
============

If you manage multiple storefronts and communities, creating sharing rules for every affected B2B Commerce custom object across every community can be a time-consuming task. To more efficiently deploy sharing rules to all of your communities, you can programmatically deploy sharing rules metadata through a command-line tool. For more information click [here](https://help.salesforce.com/articleView?id=b2b_commerce_guest_security_deploy.htm&type=5 "https://help.salesforce.com/articleView?id=b2b_commerce_guest_security_deploy.htm&type=5").

> NOTE: Deploying sharing rules metadata using the sharingGuestRules field requires API version 47.0 and later.

This document aims to help you and your team to save time adding sharing rules. This task can take a lot of time, mainly if you're managing more than one environment. For example DEV, HML, PROD environment.

All the environments need to have the same sharing rules synced. Once some of them are not, it can cause misbehavior in the storefront, and then it will be necessary to spend time to find what is wrong. Making it automatically exporting the sharing rules, can save a lot of time finding these bugs as well.

Necessary structure
=======================

To be able to export the sharing settings we will need to finish some steps:

-   Create a local, project-level folder, such as project.
-   In the project folder, create a folder to contain sharing rule metadata XML files. Name the folder sharingRules.
-   In the sharingRules folder, create a metadata XML file for each object's sharing rule. Name the file namespace__objectAPIName.sharingRules, such as ccrz__E_AccountGroup__c.sharingRules.

The structure of the project will be like this:

![](<data/img/structure.png>)

Please refer to this document: <https://help.salesforce.com/articleView?id=b2b_commerce_guest_security_deploy.htm&type=5>


Making life easier - steps to reproduce
=======================================

As a B2B Developer, I was wondering that if we have this built-in project structure already done just to export the sharing rules would be a good thing and it would save a lot of time. So, I created this repository with all guest sharing rules to be used in B2B commerce up until now. You can access [here](https://github.com/OSFGlobal/CA-OSF-RND-B2BSolutions-CCZ "https://github.com/OSFGlobal/CA-OSF-RND-B2BSolutions-CCZ") .
To export automatically the sharing rules, follow these steps:

1.  Clone the repository linked above.
2.  Open the **Exporting-Sharing-Rules** folder project with VS Code.
3.  Authorize your org. **SFDX: Authorize an Org** (you need to have Salesforce CLI installed)
    * a.  Select whether it is **Sandbox** or **Production**
4.  Find the Community Site Guest User Nickname
    a.  To be able to find the Site Guest User Nickname, follow this path in the Lightning experience
    b.  **Setup → All Communities → Workspace (Default Store) → Administration → Pages → Go to Force.com → Public Access Settings → Assigned Users → Click in the guest user → Nickname**
        ![](<data/img/guest-site-user.png>)
5.  Find the Default CC Owner Id (If it exists)
    * a.  To be able to find the Site Guest User Nickname, follow this path in the Lightning experience
    * b.  **CC Admin → DefaultStore → Configuration Settings → User Profile(Registration) → Default Record Owner**. It will be the Id of the CC Default Owner defined in [this step](https://help.salesforce.com/articleView?id=b2b_commerce_setup_guest_user_record_owner.htm&type=5 "https://help.salesforce.com/articleView?id=b2b_commerce_setup_guest_user_record_owner.htm&type=5") of installation of B2B Package.
6.  Copy the Community Site Guest User Nickname and replace it in each sharing rule metadata file defined in the project when it applies.
    * a.  Replace the <`guestUser`> tag with the Nickname found.
        ![](<data/img/replace-nickname.png>)
7.  Some sharing rules ask for the Id of the CC Default Owner User like AccountAddressBook, Cart, ContactAddress, Order, and PriceModifier sharing rules.
    * a.  Like in step 6 you need to replace the `value`field tag in the XML.
    * b.  Replace <`value`> tag with the Default CC Owner Id.
        ![](<data/img/replace-id.png>)
8.  Go to the package.xml file.
    * a.  Based on this document <https://help.salesforce.com/articleView?id=b2b_commerce_setup_sharing_rules.htm&type=5> we need some sets of sharing rules that are responsible for grant access to the Guest user. Some of them are about self-registering, add products to the cart, anonymous checkout, and so on.
    * b.  If you don't want to allow all of these sets of sharing rules you will need to comment on the code in the **package.xml** to deploy only the blocks of sharing rules necessary for your project needs.
    * c.  Once did you comment this out:
        -   Right Click in the **package.xml** file → **Deploy Source in Manifest to Orgs**

### Now you have deployed all the sharing rules you need all at once and saved a lot of time instead of adding them manually one by one.