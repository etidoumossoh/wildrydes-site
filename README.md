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

- Select your preferred region in your AWS Console
  
- First, run the aws configure in your text editor

- Create an empty git repository. Then clone the HTTPS from the clone URL to your local device. 
  
- Populate the git repository by changing your directory into your repository and then copy the static files from S3 using the following commands (make sure you change the Region in the following command to copy the files from the S3 bucket to the Region you selected earlier)
  
   *cd wildrydes-site*
  
   *aws s3 cp s3://wildrydes-eu-west-2/WebApplication/1_StaticWebHosting/website ./ --recursive*
  
- Set your Git username and email address so Git can associate your identity when pushing a commit to Github

  *git config --global user.name "yourname"*
  
  *git config --global user.email *youremailaddress*

- To Confirm that you have set Git details correctly

  *git config --global user.name*

  *git config --global user.email*

- Add, commit, and push the git files
  
  *git add .*
  
  *git commit -m "new files"*
  
  *git push*
  
- Next, we will use the AWS Amplify Console to deploy the website you've just committed to git. AWS Amplify Hosting provides a git-based workflow for hosting full-stack serverless web apps with continuous deployment.

  * Launch the AWS Amplify console.
  
  * Choose Get Started.
  
  * Under the Amplify Hosting Host, choose Get Started.
  
  * select Github and choose Continue.
 
  * Since we used GitHub, we will need to authorize AWS Amplify to your GitHub account.
 
  * On the Add repository branch step, select wildrydes-site from the Select a repository dropdown. Please note that while authorising, it is best to give the resource access       to just one repository which is our wildrydes-site
    
  * In the Branch dropdown select Main and choose Next.
    
  * On the Build settings page, leave all the defaults, select Allow AWS Amplify to automatically deploy all files hosted in your project root directory and choose Next.
    
  * On the Review page select Save and deploy.
    
  * Once completed, select the site image link underneath the thumbnail to launch your Wild Rydes site. If done correctly,  you'll see the results as seen below. 



![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/95b0e371-a032-4029-948b-4c5dcbfd9b64)


![image](https://github.com/etidoumossoh/wildrydes-site/assets/113789743/6fe91d04-c2f5-4871-9667-ea5b6c22b3be)

- It is important to note that the AWS Amplify console will rebuild and redeploy the app when it notices any changes made to the connected repository.
  
- You can modify the title name on  the index.html file then add, commit and push. Amplify Console will begin to build the site again quickly soon after it notices the update to the repository.


# STEP TWO: Manage Users

- In this section,  we'll create an Amazon Cognito user pool to manage our users' accounts. We'll deploy pages that enable customers to register as new users, verify their email addresses, and sign into the site.
  
- 


