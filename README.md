
# The `Project Brief`

*The customer, a start up known as exampleCorp is in the process of migrating their AWSPortolio website into the cloud. They have been running this application as a monolothic app for the past 10 years, on a single server with all of the code and the functionality bundled on a single server running in a server in head office.*

*For many reasons, including performance issues, cost of operating and managing the server, as well as technical debt - exampleCorp have decided to migrate this application to the cloud. Anytime they needed to make a change to the website, they would accrue many hours of downtime whilst they gained access to the web server and made their changes - and because of the applications monolothic nature, any changes were fraught with risk - could this change in one aspect of the application cause a problem in the rest of the application? This needed to change.*

*Their in house software development team have taken the application code nd separate it in the main website files (html, css, javascript for server side functionality) and split out the microservices (AWS Latest news page, Blog Post addition service, View Counter and the Contact Form) into cloudformation templates including services like Lambda, DynamoDB and Amazon Eventbridge. They have also taken the application code and put it onto an Amazon Machine Image (AMI) but at this point they are stuck. They are struggling to do two things:*

- Take the AMI and deploy an application from it which is highly scalable, fault tolerant, and well architected - using services like Application Load Balancers, CloudFront and Route 53 for DNS resolution. 

- Intergrate the microservices into the web application,without embedding the code for these serices on to the web servers themselves. 

- Do this in a secure manner - i.e. no public ip addresses on the EC2 instances, effective use of Security groups, principle of least privelage etc. 

*They have contracted you, an AWS Solutions Architect to complete the migration of the monolithic application to a well-architected application*. 

# Prequisites:

- Make sure you are in us-east-1 for this portfolio

# Step 1 - Deploy AMI

First step is to take the public ami (ami-0e4e395ccdd027dc5, you'll find it under community AMIs) and launch an instance from this AMI. If you SSH into your machine and go to the /var/www/html directory you should see there are a number of website files waiting for you to interact with:

- index.html
- index.js
- blog.html
- blog.js
- aws.html 
- aws.js
- style.css

Feel free to look around at your intial webpage by going the public ip of your instance.

This web server now has all of the server side web application code for you to star building upon, to add functionality to our webpage.

We then need to launch our 'Microservice' stacks, all of which include event driven, or serverless components which we will use to increase the functionality of our website. Feel free to look into each stack to see the components, and the lambda code to try to understand what's going on in the stack. Once they are deployed, we are going to create various endpoints (API / Function URLS) and embedd these into our application code on our EC2 instances. 

This will allow the end users to interact with our microservices in AWS, by using our website. 

Next step, simply install all the cloudformation stacks (in any order) ensuring you are in the us-east-1 region. For any parameters you are asked to include, you can write any value you like. 

# Creating Endpoints for the various microservices

# NOTES: using the chrome developer tools will be very useful when troubleshooting here - also, be wary of the fact that when you update a web server file, it may not immediately be reflected on the pages (especially wiht JavaScript files, as web browsers often cache these files) - to remove the cache right click on the refresh button within developer tools and click 'Empty cache and hard reload' - this will test any latest changes on your browser.


# Blog microservice

- Setup the HTTP API, with CORS enabled as a trigger for the FetchPost Lambda function, and ensure Access-Control-Allow-Methods = * and add this into your application code (line 6 in the file blog.js)

- After the initial stack creation, update the Upload Bucket to add the Event notifificatons for triggering the Create Post Lambda function. This sets the S3 event notification for the Lambda function and allow our microservice to work. TIP: it should only work when .txt files are uploaded, and only S3:PutObject events, triggering our 'create post ' function. 


# View Counter 

- This time, you will have to setup a Lambda function url -  with CORS enabled as a trigger for the View Counter Lambda function, and make you play around in the settings here, as there are multiple HTTP Methods involved. Expose all headers and allow all headers too (*).You will then add this into your application code (line 3 in the file index.js, simply replace the example url). You can test the function url works simply by clicking on it - if it does, add it in your index.js file. 

# contact Form

- Again, use a function URL (CORS ENABLED, Expose all headers and allow all headers too (*)) - Again simply update line 34 on index.js and watch the magic happen!


# AWS Latest news

- Again, use a function URL for the UpdateWebPage Function (CORS ENABLED, Expose all headers and allow all headers too (*)) - Again simply update line 2 and watch the magic happen! NOTE - it's worth running a manual test on the 'RSS Lambda function before you add the microservice into your webpage. This will populate the DynamoDB table with news stories - otherwise your page will be black until the eventbridge rule runs for the first time, which will be 9am UTC. 

# NOTE - if you are looking for more of a challenge, you can try to build a REST API endpoint with multiple resources and methods, so you can have a single API for all of the microservice endpoints. This does take a deeper understanding of APIs, CORS, and software architecture, - if not don't worry, the simple HTTP API / Function urls within the instructions will work too. 

# Create AMI, and build out full network architecture (VPC, ALB, SGs etc)

- Your application should be ready - you are now left on your own to follow the target architectural diagram and build a fully operational, scalable, well-architected application. Message me on Teams or lavellej286@gmail.com if you have any questions:

Happy Building! 
