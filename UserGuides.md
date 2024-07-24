# User guides

## Functional user guide

# Workplace

## I.1) Dashboards

### I.1.1) Role of a dashboard

**Dashboards** in Dynamics 365 Sales provide an overview of actionable business data that's viewable across the organization. Use **dashboards** to see important data at a glance.

Any user can build his own **dashboard** and share it to other users to give them access to it.

### I.1.2) Dashboard overview

![](images/.png) 
## I.2) Activities

### I.2.1) What is an activity?

In Dynamics 365 Sales, you use **activities** to plan, track, and organize all your customer communications.

For instance, after creating a **Campaign**, the marketing manager can define planning activities required to lunch the campaign like determining the budget of the campaign, buy all accessories regarding the **Campaign**, and so on.

### I.2.2) Create a new activity

Let us create a **Task** activity type (you can create any other type as well, like Email, Appointment, Phone call …).



# Constituents

## II.1) Create a new Organization



## II.2) Create a new Contact



## II.3) Create a new Lead

### II.3.1) What is a Lead?

It is not recommended to create **Contact** or **Organization** directly in the CRM, it is better to start creating **Leads** and then when there will be activities (phone call, email, research on that lead) with the leads, we can convert these leads into contact or account.

Creating non-relevant **Organization**s/**Contacts** in the system will pollute the value of the system.

If you get in touch with somebody in a conference and you get his business card, that business card can be either recorded as **Contact** directly or **Lead** depending on our feelings regarding that person. If he has manifested his interest for the company (has been invited in a conference and has participated to it, …), you can record that person directly as a **Contact**.

During a conference event you can distribute flyers to people and collect back their information; it is recommended to record them as **lead** in your CRM. There are several types of **leads**: _hot lead, cold lead…_; for next step you can send emails to these **leads** inviting them to connect with you; if they respond positively or click on a specific link, they can be automatically converted into **Contact**.

Every people filling a form from the web site should be recorded in the system as **Lead** first; when somebody (human or process) **qualify** that **lead** (validate that this is a real person) the process will convert that lead (it is called the **qualification process**) into either an **organization** or a **contact**.

When a **lead** exists for more than six months, the system can **disqualify** it (if you maybe try to contact the **lead** through the phone number provided, you notice it is a wrong number…). We can set an auto expiration date for leads: If not converted within 6 months, then it can be deleted.

### II.3.2) Create a new Lead record



## II.4) Create a new Opportunity

### II.4.1) What is an Opportunity?

Create an **opportunity** when your **lead** is almost ready to buy. With an **opportunity**, you can forecast _sales revenue_, set a potential _close date_, and factor in a _probability_ for the sale to occur.

Specifically, for Ren21, we can convert a **potential donor** created as **Lead** into a real **Contact** and the system can automatically create an **Opportunity** record upon the conversion process, that **Opportunity** can represent a promise of giving money to Ren21 for instance.

_Note: The system can also allow the creation of_ **_Opportunity_** _outside from the conversion process._

### II.4.2) Create a new Opportunity record



# Campaign Management

## III.1) Create a new Campaign

### III.1.1) What is a Campaign in the CRM?

In the CRM, the **Campaign** is a kind of container of different set of **activities** with different lists of audiences and with different communication channels; the idea is to have a kind of entry point where people involved will have a kind of timeline view of the project knowing that at this stage we will need these specific resources and execute these actions.

A **campaign** might be related to a project; during a project lifetime, you might have few campaigns: For the kickoff, you might need to send emails for instance …

Similarly as a project, a campaign can have a start date/ end date, objective, topic, project manager, stakeholders, budget (necessary cost of the campaign to happen), audience (who are the best people you target for the campaign), channels (communication media that will be used).

### III.1.2) Create a new Campaign record



## III.2) Quick Campaign

### III.2.1) What is a Quick Campaign in the CRM?

A **quick campaign** is used to send a single activity type (_email, phone call_, …) to a list of contacts or accounts.

We can use **quick campaign** to send direct emails to contact **marketing lists**. A **quick campaign** is created from a **marketing list**.

### III.2.2) Create a new Quick Campaign record










## III.3) Events



### III.3.1) Zoom type event

#### III.3.1.1) Create the zoom meeting


_The_ **_meeting_** _is created in Zoom after some seconds._


_And when we refresh the record, we will see new data._


#### III.3.1.2) Set the zoom meeting registration mandatory

_You can then go on Zoom App to bring more changes to the meeting details, like specifying the registration is mandatory._



_You can then copy the registration link to give it to your target._


**_Note_**_: To get the above link in the CRM you can run a flown._

#### III.3.1.3) Get the registration link in the CRM









_You can go back in Zoom to customize the_ **_registration form_**_._

#### III.3.1.4) Customize the registration form






_You can then send the_ **_registration link_** _to all invitees to the meeting and they will fill it._


#### III.3.1.5) Get the registration/participation lists from the CRM

You can get the **registration link** from the CRM, and when the meeting will take place, you can also get the participation list as well.

You just need to run the “Zoom” flow and this will happen:

1. Power automate will connect to Zoom App and retrieve the registration list
2. Power automate will connect to Zoom App and retrieve the participation list
3. All registrants will be created as Lead (if not yet leads or Contacts in the CRM)
4. All participants will be created as Lead (if not yet leads or Contacts in the CRM)
5. The system will indicate those who are both registrants and participants

Follow the below process to run the “Zoom” flow.






_After about 2 minutes, you can check the data._

**Zoom registrants**


**Zoom participants**


**Zoom registrants and participants**


### III.3.2) Non-Zoom event type

#### III.3.2.1) Event creation


#### III.3.2.2) Adding registrants




#### III.3.2.3) Adding participants




#### III.3.2.4) Get the list of registrants who re participants

To get it, you can simply run the “**Non-Zoom**” flow.

**_Note_**_: If the registrant or participant is not yet_ **_Lead_** _or_ **_Contact_** _in the CRM, the flow will create it as_ **_Lead_** _record in the CRM._







## III.4) Email templates







## III.5) Marketing list

#### III.5.1) Create a marketing list with Lead as member Type






#### III.5.2) Add Lead members to a marketing list






#### III.5.3) Synchronize Lead members from marketing list with MailChimp audience

To achieve that you just need to run “**Synch members MK list & Mailchimp**” flow.






#### III.5.4) Create a marketing list with Contact as member Type





#### III.5.5) Add Contact members to a marketing list






#### III.5.6) Synchronize Contact members from marketing list with MailChimp audience

To achieve that you just need to run “**Synch members MK list & Mailchimp**” flow.





What will happen is:

1. The system will populate exclusively **non-vip** **contacts** in the corresponding **MailChimp** **audience**.


1. The system will create a **Tag** record containing all **vip** **contacts**



The user can decide to remove or keep some **Contacts** in the **Tag** and then run “**Synch members Tag & Mailchimp**” flow and the system will add all **contacts** from the **Tag** record to the corresponding **MailChimp audience**.








## III.6) Tags



**_Note_**_: All_ **_Tags_** _recorded will automatically create a_ **_MailChimp audience_** _in MailChimp; we can add_ **_contacts_** _on the_ **_tag_** _and synchronize it with the_ **_MailChimp audience_** _to add the same_ **_contacts_** _on it; see the previous section._

# Projects & Activities

## IV.1) Projects



## IV.2) Role in Project



## IV.3) Action in Project



## IV.4) Project Task




## IV.5) Track Data Import

You can get the status of the importing process just by refreshing the import list.


# Customer Voice

## V.1) What is Customer Voice?

**Customer voice** is feedback management application/feature within **Dynamics 365** which is used to keep track of **customer metrics** that matter to your business.

## V.2) Usual process for survey

1. Creation of the survey using an app
2. Distribution of the survey
3. Those who receive the email fill the survey forms and submit it
4. The survey manager pull report on the collected forms and get some analysis of the trend
5. Take actions based on the trend


## V.3) Survey with customer voice

1. Surveys are created using templates or you can start from scratch by creating a blank survey
2. There are multiple methods to distribute the survey: email or an automated flow or a link embedded to a web page or share it through a QR code
3. Target receive and complete the survey and send it back
4. The received results are automatically collected on a report format
5. From the result you can take some actions

## V.4) Benefits of using Customer Voice

1. All parts of survey processing managed within one system
2. Automatic send of surveys without manual intervention
3. Automatically pulls reports from the responses received
4. Personalized data using variables
5. Automatic creation of actionable insights based on rules (like posting some alerts)

## V.5) Customer voice survey creation

This is the URL to have access to the survey designer:

<https://customervoice.microsoft.com/Pages/ProjectPage.aspx>


Let us assume we want to create a **survey** to collect feedback from participants to a lunch Zoom meeting for GSR 2023 project.

1. **Create a project**

Basically, a **project** can group a set of surveys. For instance, you may group all surveys related to a GSR project under a single **project**.


1. **Start from a template**

Once you create a **project**, you can now select one of the out of the box **templates**.




**_Note_**_: For working with live survey, it is under the_ **_production environment_** _you will create your surveys._


V.5.1) Rename your survey




V.5.2) Customize the survey header


### V.5.1) Survey header customization

#### V.5.1.1) Apply a style



#### V.5.1.2) Customize survey name and description



#### V.5.1.3) Theme color



#### V.5.1.4) Set a logo



#### V.5.1.5) Set a background image



### V.5.2) Create survey sections and questions

You can create different **sections** for your **survey**, section will group similar questions.




_You can rename your_ **_sections_** _accordingly._



#### V.5.2.1) Add a new question under a section






#### V.5.2.2) Apply logic to conditionally display fields









#### V.5.2.3Use advanced logic to conditionally display questions



If the company variable is equal to none then we will show the question to collect department value.



Next step will be importing contacts (while sending) then if the variable has a value, the question will now show up when the person will fill out the survey.

#### V.5.2.4) Survey message


We can customize the above message to fit our needs. For instance, you can show a message based on variable, response to a question…

So basically, we can have different kinds of message depending on the response to a question or a content of a variable.



### V.5.3) Using variables

We can use **variables** and then create custom survey to send out to people.

#### V.5.3.1) Create new variables




#### V.5.3.2) Use the variable in the form



We can also import **contacts** and **variables** to send personalized surveys.

You can download the **import template**, recover information from Dataverse to fill that file and import it back as **survey** contact file. The _regardingid_ is the record id and _regardingentity_ is the corresponding entity name.

When the **survey** will be sent, the timeline will store the information; and when the response will come back in, then we can track it in the **survey response** in the CRM.




## V.6) Satisfaction metrics

**Within the** survey **you can setup** metrics **to measure each satisfaction level of the client. Basically,** satisfaction metrics **are measurement systems you can use to measure your customer experience; This ensures that the data you collect through surveys can be analyzed successfully, and you can make decisions accordingly.**


### V.6.1) Net Promoter Score (NPS)

**NPS** is a metric used to measure customer loyalty. The score is calculated from the NPS-type question by using a scale from 0 through 10. The respondents are grouped as follows:

- **Detractors** are those who respond with a score from 0 through 6.
- **Passives** are those who respond with a score of 7 or 8.
- **Promoters** are those who respond with a score of 9 or 10.



### V.6.2) Sentiment

**Sentiment** is a metric used to identify customer sentiment toward a product or a service. Sentiment groups the responses to a text-based question as _positive, negative_, or _neutral_.


### V.6.3) Customer Satisfaction (CSAT)

**CSAT** is a metric used to measure the level of satisfaction customers have with a product or a service. **CSAT** is measured by responses to **rating-type** questions.


### V.6.4) Custom score

**Custom score** is a metric used to measure your respondent's overall satisfaction level by using survey scores. The value of this metric is generated by combining responses from multiple questions.


## V.7) Send the survey


There are several ways to achieve that:

### V.7.1) Generic link / Custom link / QR Code

This is the easiest way to send the survey.





_You can select variable you want to pass through._




**_Note_**_:_ **_Custom link_** _can help to have a unique_ **_customer voice_** _for different events; and based on the event name or the presenter, we can generate different link to collect feedbacks._

We can also download the **QR Code** specific to that link.



### V.7.2) By direct email



### V.7.3) By automation



### V.7.4) Embed a survey in a website



First, we add the code script copied from **dynamics 365 customer voice** in the website page

After that, we need to do a couple of additional things to make that survey work properly

1. Add the previous script in a tag, like a &lt;center&gt; &lt;/center&gt; tag
2. Create a div tag and add into it an additional script


You can get more detailed details here:

<https://learn.microsoft.com/en-us/dynamics365/customer-voice/embed-web-page>

### V.7.2) Sending customer survey from your own domain

We can set a friendly sender email for the survey.



We can add a custom address from **Microsoft 365 admin center** (_admin.microsoft.com_), and that email will be the one we will send out our survey from.





### V.7.3) Share survey via URL with the correct language



When we generate the **default link** and send it to people, they will be able to receive the survey in the default **language** setup in their browser setting, if not they will be able to select the language at top right corner of the **survey**.


_But for this to work you should provide a translation first._



Now what if I want to send the survey in a specified language, I can customize the link of the survey.

- After the aspx? Contained in the link, we add “lang=fr-FR&” for French language for instance.
- Since the link is too long and not friendly, we can use **Rebrandly** (_app.rebrandly.com_) to customize our URL.


## V.8) Collect feedback





_Any_ **_survey_** _feedback is automatically pulled on the reporting section._


## V.9) Using alerts

We can use alerts to automatically assign a survey inspection to a user.

**Example**: If somebody provides negative feedback a rule can be trigger and an alert sent; the alert can be assigned to somebody



## V.10) Customize HTML Email templates

You can go under the **Send** Tab and add a new **email template**, generate your own HTML code…









## Technical user guide

# Introduction

All customizations are to be done in the development environment, tested in user acceptance test environment and if all is okay pushed in the production environment; you must never make customization in the production environment directly.

# Adding a new field on the user interface

Let us assume we want to add a new field on the contact form specifically on the Summary Tab.

The text field will be called “Notes” and it will be added under the Ren21 Account manager field, see the below screenshot.


Follow the below steps to add the field:

## II.1) Go in the development environment

From Power app designer (_make.powerapps.com_)


## II.2) Go in the right solution

The solution where all customization will be done is “Ren21FundraisingandEngagement”.



## II.3) Add the field on the Contact entity







The field is now created.

Next step is to add the field on the form.

## II.4) Add the field on the form


The right form is “Fundraising: Contact REN21”





## II.5) Refresh the app from the development to see changes

Use shortcut (CTRL + F5) to empty the cache

You can do it twice or trice, until you see the change.


Next step is to export the solution from development environment to import it in the user acceptance test environment.

## II.6) Exporting the solution







The solution is exported as Zip file


Next step will be to import that Zip file in the user acceptance test (for testing purpose), and if okay import that file in the production environment.

## II.7) Importing the solution






Once the solution is imported, you will be able to see the new field.

# Adding a new value to an option set

We will add a new value (“Contractor)” to the contribution ro choice values

Follow steps one and two from previous section to go in the right environment and select the correct solution.





Follow steps six and seven of the previous section for exporting/importing the solution

# Create power automate flow

We will build a very simple flow and explain all the steps in details.

This is what we want to automate: Whenever a contact is updated, and email notification must be sent to a specific email address (“<thomas.andre@ren21.net>”).

## IV.1) Go in the power automate flow designer

From Power automate designer (make.powerautomate.com)


## IV.2) Go in the right solution

The solution where all customization will be done is “Ren21FundraisingandEngagement”.



## IV.3) Create the flow













## IV.4) Test the flow





Go back and see the flow execution output


The above shows that all is working well, the email has been well sent.

Next step is to export the solution and import it in the user acceptance test environment still for testing purpose; when okay, the solution will be imported in the production environment.

Refer to section II) to see how to export/import solution.

# Import data using Excel templates

The below will show how to import Roles in Project record, the logic is the same for importing other kind of data.





You can double check the column mapping by clicking “Review Mapping” button.
