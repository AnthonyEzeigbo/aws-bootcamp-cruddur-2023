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


. In the <ins>Hosted authentication pages</ins> section, DON'T check the box that says Use the `Cognito Hosted UI`. for this project it wont be needed.

. In the Initial app client section, select `Public client` as our App type.

. For the App client name; enter cruddur since the name of our app is cruddur. After that, leave the rest of the default selections and click Next


![](assets%20week3/Amazon%20cognito/amazon%20cognito%208.PNG)



**Step NO 6:**

. This section, is where you get to  review all our configurations and then go ahead to Create user pool. After creation, you should receive a success message.

![](assets%20week3/Amazon%20cognito/amazon%20cognito%209.PNG)






![](assets%20week3/Amazon%20cognito/amazon%20cognito%2010.PNG)




2. ### Configure Amazon Amplify

***What is <ins>Amazon Amplify</ins>***


Amazon Amplify is a complete solution that lets frontend web developers and mobile developers to easily build, ship, and host full-stack applications on AWS, with the flexibility to leverage the breadth of AWS services as use cases evolve, It is a full-stack application platform that is a combination of both client-side and server-side code, if you want to learn more about Amazon Amplify here is a [LINK](https://aws.amazon.com/amplify/#:~:text=AWS%20Amplify%20is%20a%20complete,Build%20a%20frontend%20UI).


How configure our Amazon Amplify.


**Step 1:**
Dont forget that development environment is still `gitpod`, while your in your Dev environment In your Terminal, `cd` into the `frontend-react-js` directory, and run this command:

```
# navigate to frontend-react-js
cd frontend-react-js

# add aws-amplify library
npm i aws-amplify --save
# the --save flag saves the library to your package.json file 
```


Navigate into your `frontend-react-js/src/App.js` and add the following contents. Make sure to add it before the `router` configuration.


```
// AWS Amplify
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```

**Step 2:**

 We will now have to set our environment variables from the code mentioned above. Go into your `docker-compose.yml` file, and under the `frontend-react-js` service, by add the following lines of code below:

```
# AWS Amplify
REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID: "${REACT_APP_AWS_USER_POOLS_ID}"
REACT_APP_CLIENT_ID: "${REACT_APP_CLIENT_ID}"
```
**You will have to locate your `user pool  ID`, because it will have to be inserted in the `REACT_APP_CLIENT_ID` section of the code above.
**The user pool name will be found in your `Amazon cognito` in the AWS console, click into the pool you created and look for the `App integration tab` as shown in the image below. Then scroll all the way down to view your `client ID`**

![Amazon Cognito Dash Board](assets%20week3/amazon%20cognito%2012.PNG)

**Step 3:**


The follwoing lines of code as to be added in the `frontend-react-js/src/pages/HomeFeedPage.js` file.


```
// AWS Amplify
import { Auth } from 'aws-amplify';

// DELETE THESE LINES
const checkAuth = async () => {
    console.log('checkAuth')
    // [TODO] Authenication
    if (Cookies.get('user.logged_in')) {
      setUser({
        display_name: Cookies.get('user.name'),
        handle: Cookies.get('user.username')
      })
    }
  };


// ADD THESE LINES 
// check if we are authenticated
const checkAuth = async () => {
  Auth.currentAuthenticatedUser({
    // Optional, By default is false. 
    // If set to true, this call will send a 
    // request to Cognito to get the latest user data
    bypassCache: false 
  })
  .then((user) => {
    console.log('user',user);
    return Auth.currentAuthenticatedUser()
  }).then((cognito_user) => {
      setUser({
        display_name: cognito_user.attributes.name,
        handle: cognito_user.attributes.preferred_username
      })
  })
  .catch((err) => console.log(err));
};
```

**Step 4:**

Using the following line of codes update the `frontend-react-js/src/components/ProfileInfo.js` file

```
// DELETE THIS LINE 
import Cookies from 'js-cookie'

//ADD THIS LINE
// AWS Amplify
import { Auth } from 'aws-amplify';

// DELETE THESE LINES 
const signOut = async () => {
    console.log('signOut')
    // [TODO] Authenication
    Cookies.remove('user.logged_in')
    //Cookies.remove('user.name')
    //Cookies.remove('user.username')
    //Cookies.remove('user.email')
    //Cookies.remove('user.password')
    //Cookies.remove('user.confirmation_code')
    window.location.href = "/"
  }

// ADD THESE LINES 
const signOut = async () => {
  try {
      await Auth.signOut({ global: true });
      window.location.href = "/"
  } catch (error) {
      console.log('error signing out: ', error);
  }
}
```
