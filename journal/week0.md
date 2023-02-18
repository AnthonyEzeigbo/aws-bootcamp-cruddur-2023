# Week 0 — Billing and Architecture


## Required HomeWork 

This is my week 0 journal, i made sure all my working environment was setup completely 


### AWS Console
I made sure to setup IAM user after creating my AWS account,using the IAM user account is considerd best practice.
My ROOT user account is only going to be used to make payments and close the account.

### AWS Security 
To proparly secure my AWS account i had to enable MFA to hardern security on my IAM user and my ROOT account
I also had to create an iamadmin user in my IAM user account, i had to attache my iamadmin user to a group named admin in which full accsess,
And previllages are assigned to them both 

### AWS Budget
I setup two alarms one for budget-spent and the other for credit-spent,should incase i go beyound the free tier perimeter and the amount i set in place,
So that i can be notified and take action on the account to avoid been charged 

![AWS BUDGET](asset/AWS%20BUDGET%20SNIP.PNG) 

### AWS Organization

I learnt about AWS Organization and how it works, its an account management that helps you consolidate multiple AWS account into one organization,
That is been created by you and that you centrally manage.

### GITPOD -> GITHUB

I was able to use gitpod for the first time/i created a free gitpod trial account using my github account, to sync gitpod on github i had to,
Download gitpod extention on my chrome browser so anytime i open github gitpod opens as well and one click on the gitpod pop up it takes me to a
Vscode IDE on browser, i typed in all the linux command and downloaded the zip file from AWS install documentation.

### NAPKIN DIAGRAM

![Napkin architectural diadram](asset/napkin%20diagram.jpeg)

### PHYSICAL DIAGRAM

![CRUDDUR Logical Architectural Design](asset/physical%20diagram%20snip.PNG)

[Link to the diagram](https://lucid.app/lucidchart/c833603e-14b9-4b45-8e31-42c22ebc0ab1/edit?viewport_loc=2%2C63%2C3072%2C1512%2C0_0&invitationId=inv_18f9e114-800d-4cfc-bb71-ac5cb3efeb09)

