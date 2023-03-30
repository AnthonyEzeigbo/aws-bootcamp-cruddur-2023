# Week 3 — Decentralized Authentication

## Required Homework

You will have to use `Gitpod` workspace  and  `AWS` console for this homework


1. ### Setting up AWS Cognito User Pool


Firstly lets understand what Amazon Cognito is and what it does

Amazon Cognito provides authentication, authorization, and user management for your web appliction, eg facebook and mobile application, eg whatsapp. Your users can sign in directly with a username and password or through a third party such as Facebook, Amazon, Google, or Apple. there are several components attached to Amazon Cognito one being `AWS COGNITO USER POOL`  and `AWS COGNITO USER IDENTITY` We will be working the  user pool.

`User pools` are simply user directories that provide sign-up and sign-in options for your mobile app and web app  users, you can learn more about this  service in the [AWS Documentation](https://docs.aws.amazon.com/).


### How to setup your AWS cognito userpool using the aws console 

login to your AWS account and search for `Cognito`
select the first option that says, `Amazon Cognito`.


The following steps are applied

**Step NO 1:**

  . In the area that says `Provider types` section look below it ,  select `Cognito user pool`:
  
  
   . Once your in the `Cognito user pool` sign-in options,  select  the `User name` and `Email` attributes. DON'T check any of  the  boxes for the User name requirements. Click Next



***You have to select the highlighted or circled areas***

![Amazon cognito](assets%20week3/Amazon%20cognito/amazon%20cognito%201.PNG)






***You have to select the highlighted or circled areas***

![User Pool setup](assets%20week3/Amazon%20cognito/amazon%20cognito%202.PNG)



***check the user name and email***

![](assets%20week3/Amazon%20cognito/amazon%20cognito%204.PNG)




**Step NO 2:**

. In the <ins>Password policy</ins> section,  for this project we will be using the `Cognito defaults`,  you can  customize  yours to fit whatever project your working on.

.In the section that says Multi-factor authentication. We won't be using any MFA because this service is not included in the free tier. But, if you want to secure your appliction even more then add an MFA,but be adviced that it comes with its cost see [pricing](https://aws.amazon.com/getting-started/hands-on/setup-email-receiving-pipeline/services-costs/) before implementation.




![](assets%20week3/Amazon%20cognito/amazon%20cognito%205.PNG)




. In the <ins>User account recovery</ins> section,  the box that says `Enable self-service account recovery` should be checked. with that feature been checked it will  enables users to reset password 


. please select the `Email only` option. this feature is part of the free tier . If you want to use the SMS option beware that it does comes with a cost, continue by  Clicking Next.



![](assets%20week3/Amazon%20cognito/amazon%20cognito%205%20continues.PNG)





**Step NO 3:**

. In the <ins>Self-service sign-up section</ins>, make sure you check the box that says  `Enable self-registration`.

![](assets%20week3/Amazon%20cognito/amazon%20cognito%206.PNG)



. Assuming we,ve been following the steps,the next thing will be the  `Attribute verification and user account confirmation section`. leave the default selections as is. 




![](assets%20week3/Amazon%20cognito/amazon%20cognito%206.PNG)


In the <ins>Required attributes section</ins>, add additional required attributes that yoy would like your app to have for this app, we will be adding - name as one of the attributes




**Step NO 4:**


In the <ins>Email provider section</ins>,  select to Send email with Cognito only, and allow the default selection as is in  the option.to continue click next.



![](assets%20week3/Amazon%20cognito/amazon%20cognito%207.PNG)



**Step NO 5:**


. The <ins>User pool name section</ins>, will need to have a name and make sure the names are not generic, create a freindly user pool name.

Once you chose that name and it has been created the name cant be rechanged once created but the attrinutes can change.


![](assets%20week3/Amazon%20cognito/amazon%20cognito%208.PNG)


