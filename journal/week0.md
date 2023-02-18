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
