# Project 3 Overview
Project 3 has 2 phases.
Phase 1 - Create any multi page website on AWS S3 with static hosting and CloudFront

This enables you to create a mostly static website, one that would be useful for disseminating information. 6949 companies reportedly use Amazon S3 in their tech stacks, including Airbnb, Pinterest, and Netflix.
Phase 2 - Create a serverless website setup and code repositories.

This enables you to create dynamic websites that can have information pushed to a repository, like Git, and the data becomes available anywhere on the web. 2591 companies reportedly use AWS Lambda in their tech stacks, including Udemy, Delivery Hero, and Nubank.

###  Phase 1 - Static Website Hosting

1. Launch a Multi-Page Website on AWS S3
    Use Amazon S3 for static hosting. Popular use cases include informational websites.
    Companies like Airbnb and Netflix utilize S3 for similar purposes.

2. Secure the Website with CloudFront and SSL Certificates
    Integrate CloudFront for caching and content distribution.
    Secure the site with Route 53 (DNS management) and AWS Certificate Manager (SSL certificates).
    Set up hosted zones, A records, and name servers for DNS validation through Namecheap.

3. Test and Finalize Secure Setup
    Verify the CloudFront distribution and secure access to the website.
    Use a custom domain provided through Namecheap, such as CS412+initial+lastname.me.

### Phase 2 - Serverless and Dynamic Features

1. Set Up GitHub Repository
    Create a GitHub repository to house the website code.
    Push the website’s files from the local environment to the GitHub repo using Git commands.

2. Automate Updates with AWS CodePipeline
    Use AWS CodePipeline to establish continuous deployment:
        Source: GitHub repository.
        Build: No specific build stage required for static content.
        Deploy: AWS S3 bucket hosting the static website.

3. Stop Insecure Access
    Restrict public access by:
        Using Origin Access Identity (OAI) for CloudFront to access the S3 bucket.
        Updating the bucket policy to deny all public requests and allow only CloudFront access.

### Key Steps for Success

- Cost Efficiency: The average project cost is around $1.59, but ensure resources are cleaned up post-project to avoid unexpected charges.
- Best Practices:
    Name repositories and domains consistently for easy tracking.
    Test DNS propagation and SSL issuance thoroughly; delays might occur.
- Use Case Relevance: Incorporate Lambda functions or other serverless features if dynamic functionality is needed.

## Project 3 - Phase 1 Part 1 Demo
Launching your website using AWS S3 Bucket

### Steps to follow: (Brief)

[Last edited on December 14 , 2024]  

#### Step 0: Buy your own domain

 - Follow the instruction by Professor Robert Lightfoot to buy a domain for yourself at Namecheap

Links to an external site.. (This link is for a free student account.)

#### Step 1: Create your Webpage

 - On your local machine, create a static webpage in a folder of any name (example: 'website') .
 - You may use  your CSCE 331 web page. If that isn't available, use the following as a sample for this demo: static-webpage-example.zip.

Download static-webpage-example.zip.Open this document with ReadSpeaker docReader

#### Step 2: Log in to AWS

 - Log in to your AWS console.
 - Search and click on S3 to access S3 services.

#### Step 3: Create a new Bucket

 - Provide a name (Should match your domain. Example: CS412RLightfoot.me).
 - Choose a region (Keep default for now - "(Ohio) us-east-2").
 - Enable Bucket Versioning (can be configured/changed later).
 - Block all public access (can be configured/changed later).
 - The rest can stay as is.

#### Step 4: Upload web content to your AWS Bucket 

 - Click on the created Bucket from the list (There should be at least one in the list).
 - Upload the contents from the 'website' folder by "drag-drop" or "Add folder" or "Add files" method.
 - Click on the properties tab and select the "Standard" option for 'storage-class'.
 - Click upload to complete the upload.
 - Click close to go back to your Bucket dashboard.

#### Step 5: Host the Bucket as a Static Website

 - Click on the Properties tab.
 - Select Edit for "Static website hosting".
 - Select Enable for "Static website hosting".
 - Enter the filename in the index document field.
 - Keep the rest as is.
 - Save Changes.

#### Step 6: Configure Bucket Permissions 

 - Go to the Permission tab of your Bucket.
 - Edit "Block all public access" and uncheck "Block all public access".
 - type confirm if asked then save/complete.
 - Add a bucket policy - click here for a basic policy (Replace the bucket name with yours).
 - Edit "Object Ownership" and check 'ACLs enabled' with "Bucket owner preferred".
 - (type confirm if asked) Save Changes.
 
#### Step 7: Make the bucket contents Public 

 - Go back to the Object tab of your Bucket.
 - Select your uploaded contents (Files and folders uploaded from 'website') by clicking the checkboxes.
 - Select Actions - Make Public Using ACL.

#### Step 8: Verify public access

 - Click on the uploaded folder.
 - Click on the 'index.html' file and copy the object URL.
 - Save it on a notepad file for use in a Browser.
 - Go back to the Bucket dashboard and click on the properties tab to copy the "Endpoint" URL.
 - Save it on a notepad file for use in Step 9.
 - Use both links in a browser to verify public access to the website.
 
#### Step 9: Bind your domain with S3 Bucket

 - Log in to your domain host's CPanel (Example - Namecheap CPanel).
 - Go to Host Records.
 - Add New Record, select CNAME Record.
 - Type "www" or "your-subdomain-name" if your bucket has "www" or any other subdomain (e.g., "info") in the Host field and copy/paste the 'Endpoint' URL without the "http://" part in the "Value" field.  If the bucket does not have a subdomain, just add "@" in the Host field.
 - Choose a custom TTL for your site (Example: 5 mins).

## Project 3 - Phase 1 Part 2 Demo
Making your website secure with SSL certificate

### Steps to follow: (Brief)

This demo involves activities involving Namechep.com, AWS S3, AWS CloudFront, AWS Route 53, and AWS Certificate Manager. 

[Last edited on - Dec 16, 2024]  

#### Step 0: Launch your first website 

 - Follow the instructions on Project 3 - Phase 1 Demo page and launch your first static website.
 - This website does not require a secure connection!

#### Step 1: Setup a Hosted Zone on Route 53

 - Go to Route 53 Console on AWS: (https://console.aws.amazon.com/route53/home

Links to an external site.) .
 - Click on "Create a hosted zone".
 - On "Domain name", write your domain's name exactly as it was registered (e.g., CS412RLightfoot.me).
 - keep the selection for "public hosted zone".
 - finish creating hosted zone.
 - Inside the created hosted zone (e.g., csce412-your-name.xyz) click on the "Hosted zone details".
 - It will provide you with 4/5 "Name servers" on the right.
 - Make a note on those name servers to be used in a later step.
 - Click on "Create a record" and choose "switch to wizard" on the top right.
 - Select "Simple routing" - "next" - "Define Simple Record".
 - For "Record name", if your bucket has a subdomain (like www.csce412-your-name.xyz), type the subdomain (e.g., www) in the blank space otherwise leave it blank.
 - Select "Record Type" as "A - Routes traffic ...." - (Important for certificate verification in A later step).
 - In the "Value" field, choose "Alias to S3 website endpoint" - Select your zone - select your bucket (bucket name should match record name).
 - Click "Define Simple Record" at the bottom right - then click "Create record" to finish it.
 - Verify that the record is listed on your hosted zone.

#### Step 2: Add Nameservers on Namecheap 

 - This step is ALSO important for certificate validation.
 - Log in to your Namecheap console. (https://www.namecheap.com/

Links to an external site.).
 - Click on "manage" to manage your domain.
 - On "Nameservers", select "Custom DNS" and add the Nameservers from Step 1.
 - Once you finish, your website may become unavailable temporarily.

#### Step 3: Create a certificate on ACM

 - Go to the AWS certificate manager console (Make sure you are on "N-Virginia us-east-1" this time) (https://console.aws.amazon.com/acm/home?

Links to an external site.).
 - Click "Request a certificate".
 - add your domain name in the "Fully qualified domain name" field (like CS412RLightfoot.me).
 - Click "add another name" and enter (*) as subdomain with the domain (like *.CS412RLightfoot.me).
 - Select "DNS Validation", click "Request" then refresh your page.
 - The status should say "Pending Validation".
 - Click on the certificate and click on "Create records in Route 53".
 - Choose available records and click on "create records".
 - Wait for the status to become "issued", you may need to refresh several times. (Previous A record and NS entries! were done for this).
 - Once issued, you have a valid certificate for your domain.

#### Step 4: Create a CloudFront Distribution

 - Go to the AWS CloudFront Console (https://console.aws.amazon.com/cloudfront/v3/home?

Links to an external site.).
 - Click on the "Create Distribution".
 - On "Original domain", Select your S3 domain (it should show up once you click on it).
 - Leave "Original path" as blank as our index.html file is in the root folder of the bucket.
 - Keep "Origin access" as public and select your "Viewer protocol policy" - (e.g., Redirect HTTP to HTTPS).
 - Scroll down on "Custom SSL Certificate" and choose your created certificate.
 - If your bucket name has a subdomain like "www" before your domain "CS412RLightfoot.me" follow the next two "(Optional)" steps below.
 - (Optional) Under Settings, on "Alternate domain name (CNAME) - optional", click on "Add item".
 - (Optional) Type your bucket name with the subdomain on the entry field (e.g., "www.CS412RLightfoot.me").
 - On "Supported HTTP versions" select both (Optional for some websites).
 - On "Default root object", type index.html.
 - You can keep all other configurations as default.
 - Click "Create distribution" and wait for the deployment to complete. (It may take a while).
 - Once deployed, copy the "Distribution domain name", make a note of it, and test it on a browser to verify.
 - On success, this should establish a secure connection to your website!

#### Step 5: Create/Replace A Record on Route 53

 - Go to your hosted zone on Route 53 (https://console.aws.amazon.com/route53/home

Links to an external site.) .
 - Delete the previously created A Record.
 - Create a new A Record like before (in Step 1)
 - This time in the "Value" field choose "Alias to CloudFront Distribution".
 - If you click on the search bar below, it should show your distribution, select it OR, if not showing follow the "(Optional)" step below.
 - (Optional) You can copy and paste the "Distribution domain name" without the "http://" part here.
 - Click on "Define Simple Record" at the bottom right - then click on "Create record" to finish it.

Now, within a few minutes, you should have access to a securely connected website of your own.