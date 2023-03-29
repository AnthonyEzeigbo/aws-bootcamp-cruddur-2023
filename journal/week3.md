# Week 3 — Decentralized Authentication

## Required Homework

You will have to use `Gitpod` workspace  and  `AWS` console for this homework


1. **Setting up AWS Cognito User Pool** 


Firstly lets understand what Amazon Cognito is and what it does

Amazon Cognito provides authentication, authorization, and user management for your web appliction, eg facebook and mobile application, eg whatsapp. Your users can sign in directly with a username and password or through a third party such as Facebook, Amazon, Google, or Apple. there are several components attached to Amazon Cognito one being `AWS COGNITO USER POOL`  and `AWS COGNITO USER IDENTITY` We will be working the  user pool.

`User pools` are simply user directories that provide sign-up and sign-in options for your mobile app and web app  users, you can learn more about this  service in the [AWS Documentation](https://docs.aws.amazon.com/).


**How to setup your AWS cognito userpool using the aws console** 

login to your AWS account and search for `Cognito`
select the first option that says, `Amazon Cognito`.


The following steps are applied

**Step NO 1:**

    . In the area that says `Provider types` section look below it ,  select `Cognito user pool`:
    . Once your in the `Cognito user pool` sign-in options,  select  the `User name` and `Email` attributes. DON'T check any of  the  boxes for the User name requirements. Click Next
