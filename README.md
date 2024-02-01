# wildrydes-site
# Building a Scalable Ride-Sharing Web Application on AWS

This project focuses on building an end-to-end application comprising 6 AWS services namely:
- AWS Amplify
- Amazon Cognito
- Dynamo DB
- LAMBDA
- API Gateway

The web application allows registered users to request registered unicorn rides from the WildRydes fleet. 

# Pre-requisites
- An AWS account
- AWS CLI installed on your device
- ArcGis account
- Text Editor. I used VSCode

# STEP ONE: Deploy a Static Website

## AWS Configuration

- Select your preferred region in your AWS Console
  
- First, run the aws configure in your text editor

## Create a Git Repository and Populate Same

- Create an empty git repository. Then clone the HTTPS from the clone URL to your local device. 
  
- Populate the git repository by changing your directory into your repository and then copy the static files from S3 using the following commands (make sure you change the Region in the following command to copy the files from the S3 bucket to the Region you selected earlier)
  
   ****cd wildrydes-site*
  
   ****aws s3 cp s3://wildrydes-eu-west-2/WebApplication/1_StaticWebHosting/website ./ --recursive*
  
- Set your Git username and email address so Git can associate your identity when pushing a commit to Github

  ****git config --global user.name "yourname"*
  
  ****git config --global user.email *youremailaddress*

- To Confirm that you have set Git details correctly

  ****git config --global user.name*

  ****git config --global user.email*

- Add, commit, and push the git files
  
  ****git add .*
  
  ****git commit -m "new files"*
  
  ****git push*

  ## Enable Web Hosting with AWS Amplify

-  Next, we will use the AWS Amplify Console to deploy the website you've just committed to git. AWS Amplify Hosting provides a git-based workflow for hosting full-stack serverless web apps with continuous deployment.
   
   -  Launch the AWS Amplify console.
   
   -    Choose Get Started.
   
   -  Under the Amplify Hosting Host, choose Get Started.
   
   -    select Github and choose Continue.
   
   -  Since we used GitHub, we will need to authorize AWS Amplify to your GitHub account.
   
   -  On the Add repository branch step, select wildrydes-site from the Select a repository dropdown. Please note that while authorising, it is best to give the resource             access to just one repository which is our wildrydes-site

   -  In the Branch dropdown select Main and choose Next.
   
   -  On the Build settings page, leave all the defaults, select Allow AWS Amplify to automatically deploy all files hosted in your project root directory and choose Next.
   
   -    On the Review page select Save and deploy.
   
   -    Once completed, select the site image link underneath the thumbnail to launch your Wild Rydes site. If done correctly,  you'll see the results as seen below. 



![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/95b0e371-a032-4029-948b-4c5dcbfd9b64)


![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/6fe91d04-c2f5-4871-9667-ea5b6c22b3be)

-  It is important to note that the AWS Amplify console will rebuild and redeploy the app when it notices any changes made to the connected repository.

-  You can modify the title name on  the index.html file then add, commit and push. Amplify Console will begin to build the site again quickly soon after it notices the update to the repository.


# STEP TWO: Manage Users


## Create an Amazon Cognito User Pool 


- In this section,  we'll create an Amazon Cognito user pool to manage our users' accounts. We'll deploy pages that enable customers to register as new users, verify their email addresses, and sign into the site.

    - In the Amazon Cognito console, choose Create user pool.
    
    - On the Configure sign-in experience page, select User name. Keep the defaults for the other settings. Choose Next.
    
    - On the Configure security requirements page, keep the Password policy mode as Cognito defaults.
    
    - On the Configure sign-up experience page, keep everything as default. Choose Next.
    
    - On the Configure message delivery page, for Email provider, choose Send Email with Cognito.
    
    - On the Integrate your app page, name your user pool: WildRydes. Under Initial app client, name the app client: WildRydesWebApp and keep the other settings as default.
    
    - On the Review and Create page, choose Create user pool.
    
    -   On the User Pool page, select the User Pool name to view detailed information about the user pool you created. Copy the User Pool ID in the User Pool overview section and save it in a secure location on your local machine.

  ![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/cafe0cf9-ee15-4445-9f14-d82b85b6ba12)

   - Select the App Integration tab and copy and save the Client ID in the App Clients and analytics section of your newly created user pool.

## Update the Website Config File


- From your local machine, open the wild ride-site/js/config.js file in a text editor and update the cognito section of the file with the correct values for the User pool ID and App Client ID you saved.

-  Save the modified file.

- In your terminal window, add, commit, and push the file to your Git repository to have it automatically deploy to Amplify Console.

## Validate your Implementation

- In your web browser, navigate to the wildrydes-site folder you copied to your local machine in Module 1.

- Open /register.html, or choose the Giddy Up! button on the homepage (index.html page) of your site.

- Complete the registration form and choose Let's Ryde. You can use your email.  Make sure to choose a password that contains at least one upper-case letter, a number, and a special character. You should see an alert that confirms that your user has been created.

- If you used an email address you control, you can complete the account verification process by entering the verification code that is emailed to you. Please note, that the verification email may end up in your spam folder.

- After confirming the new user by using the email address and password you entered during the registration step. If successful you should be redirected to /ride.html. You should see a notification that the API is not configured. Important: Copy and save the auth token to create the Amazon Cognito user pool authorizer in the next module.

  ![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/f4f4ec23-31a4-4f95-a5c9-b634b8f24582)

# STEP THREE: Build a Serverless Backend 

## Create an Amazon Dynamo DB 

- First, we create a Dynamo DB. In the Amazon DynamoDB console, choose Create table.

- For the Table name, enter Rides.

- For the Partition key, enter RideId and select String for the key type.

- In the Table settings section, ensure the Default setting is selected, and choose Create table.

- On the Tables page, wait for your table creation to complete.

- Once it is completed, the status will say Active. Select your table name.

- In the Overview tab > General Information section of your new table, choose Additional info. Copy the ARN. You will use this in the next section.

![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/76c830ef-91a2-49cf-9588-acd3b52807a1)

## Create an IAM Role for our LAMBDA Function

- We'll need to create an IAM role that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB

- In the IAM console, select Roles in the left navigation pane and then choose Create Role.

- In the Trusted Entity Type section, select AWS service. For the Use case, select Lambda, then choose Next.

- Enter AWSLambdaBasicExecutionRole in the filter text box and press Enter.

- Select the checkbox next to the AWSLambdaBasicExecutionRole policy name and choose Next.

- Enter WildRydesLambda for the Role Name. Keep the default settings for the other parameters. Choose Create Role.

- In the filter box on the Roles page type WildRydesLambda and select the name of the role you just created.

- On the Permissions tab, under Add Permissions, choose Create Inline Policy.

- In the Select, a service section, type DynamoDB into the search bar, and select DynamoDB when it appears. Choose Select actions.

- In the Actions allowed section, type PutItem into the search bar and select the checkbox next to PutItem when it appears.

- In the Resources section, with the Specific option selected, choose the Add ARN link.

- Select the Text tab. Paste the ARN of the table you created in DynamoDB (Step 6 in the previous section), and choose Add ARNs.
Choose Next.

- Enter DynamoDBWriteAccess for the policy name and choose Create policy.

  ![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/d916e5bd-cf55-4fc3-bfe1-160d94ccf5c2)

## Create a LAMBDA Function

- Next, we will create a Lambda function for handling requests. AWS Lambda will run our code in response to events such as an HTTP request. In this step, we'll build the core function that will process API requests from the web application to dispatch a unicorn.

- From the AWS Lambda console, choose Create a function.

- Keep the default Author from scratch card selected.

- Enter RequestUnicorn in the Function name field.

- Select Node.js 16.x for the Runtime (newer versions of Node.js will not work in this tutorial).

- Select Use an existing role from the Change default execution role dropdown.

- Select WildRydesLambda from the Existing Role dropdown. Click on the Create function.

- Scroll down to the Code source section and replace the existing code in the index.js code editor with the contents of requestUnicorn.js.

![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/0a53fc57-e6ad-4c65-9a12-f55c9c286fc5)

- Then choose Choose Deploy.

## Test Implementation 

- Next, we will test the function that you built using the AWS Lambda console. In the RequestUnicorn function you built in the previous section, choose Test in the Code source section, and select Configure test event from the dropdown.

- Keep the Create new event default selection.

- Enter TestRequestEvent in the Event name field.

- Copy and paste the following test event into the Event JSON section:

  `{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}`

- Choose Save.

- In the Code source section of your function, choose Test and select TestRequestEvent from the dropdown.

- On the Test tab, choose Test.

- In the Executing function: succeeded message that appears, expand the Details dropdown.

- Verify that the function result looks like the following:


![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/5ab9a5a0-5dfb-49ed-a539-0c0121b16f53)



# STEP FOUR: Deploy a RESTful API

## Create a Rest API

- In the Amazon API Gateway console, select APIs in the left navigation pane.

- Choose Build under REST API. 

- In the Choose the Protocol section, select REST.

- In the Create new API section, select New API.

- In the Settings section, enter WildRydes for the API Name and select Edge Optimized in the Endpoint Type dropdown.

- Choose Create API.

![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/6f20cdd2-634b-4db9-84e5-822ebbf04329)

## Create an Authorizer

- In this section, we will be creating an Authorizer for the API, so we can make use of the user pool.

- Use the following steps to configure the Authorizer in the Amazon API Gateway console:

- In the left navigation pane of the WildRydes API you just created, select Authorizers.
  
- Choose Create New Authorizer.

- Enter WildRydes into the Authorizer Name field

- Select Cognito as the Type.

- Under Cognito User Pool, in the Region drop-down, select the same Region you have been using for the rest of the tutorial. Enter WildRydes in the Cognito User Pool name field.

- Enter Authorization for the Token Source.

- Choose Create.
  
- To verify the authorizer configuration, select Test.

- Paste the Authorization Token copied from the ride.html webpage and verify that the HTTP status Response code is 200.

![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/b36d7636-6dec-454a-8fb6-5079a1f79907)

 
