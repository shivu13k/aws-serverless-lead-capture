# Phase 1: Hosting Static Website
## Step 1: Download Website
Clone project from github in powershell:
git clone https://github.com/pravinmishraaws/Ebook.git

## Step 2: Locate downloaded file in your terminal
ls

## Step 3: Create S3 bucket:
aws s3 ls
aws s3 mb s3://epicreads-shivani

## Step 4: edit static website hosting:
in your s3 bucket properties, enable static website hosting. When you try to see your site using the website endpoint mentioned the website hosting tab, you'll still see 403 Forbidden page.

## Step 5: Make bucket public

In S3 bucket permissions tab, make the disable block public access.

## Step 5: Add Bucket policy:
In permissions tab, add policy:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}

## Step 7: Display error page
in powershell:
New-Item 404.html
code 404.html
this opens the file in vscode, add the following html code:
<html>
<head>
    <title>Error Page</title>
</head>
<body>
    <h1>Hi I am an error page!</h1>
    <p>Sorry, I will be back soon!</p>
</body>
</html>

and save it.

Then in powershell, copy local file 404.html to s3 bucket using:
aws s3 cp 404.html s3://epicreads-shivani

## Step 8: Sync all files (html, css etc) to s3:
aws s3 sync . s3//epicreads-shivani
Can verify the upload in console

## Step 9: Refresh browser and view website

## Step 10: Get Public Certificate in us-east region
First you should have a domain, if not buy one using Route53, it will create 1 hosted zone and 2 records - NS and SOA. Then in Certificate manager, create one certificate for your domain and create a record for CNAME, it will issue your certificate and make your validation successful.

## Step 11: Cloudfront (global)
Create a cloudfront distribution, add your domain name to Distribution name (remove the '.com').
add domain name and check if all good. Then proceed to next.

## Step 12: Route53 settings
In Route53 hosted zone, select your zone, go to records and add A type record by creating record, enable alias and select route traffic to endpoint cloudfront. once it is in sync your domain is connected to the cloudfront distribution and you should be able to access your domain site.