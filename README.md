# Creating-and-Configuring-Amazon-S3-Buckets-to-Host-a-Static-Website-with-a-Custom-Domain
Create S3 buckets and static website.
Navigate to the S3 portion of the console
In another tab, navigate to the Route53 portion of the console
Find the domain name inside the HostedZone
Create a bucket that matches the domain name, such as cloudlab240.info
Also create a bucket that is the same except that it has a www. prefix (e.g., www.cloudlab240.info. The bucket name MUST be identical to the domain name).
Make the buckets public (NOTE: AWS has changed how Public Access is edited for S3. See https://docs.aws.amazon.com/AmazonS3/latest/user-guide/block-public-access-bucket.html )
Upload the website files to the bucket
Make the website files public
Configure one of the buckets as a static website
Configure the other bucket to redirect to the static website

Configure a custom domain in Route 53.
Configure a www and non-www custom domain to point to your AWS S3 static site:

Navigate to Route 53
Inside the hosted zone for your domain, press create record
Make sure to select an A record to create
The first A record should be for the non-www domain (without anything additional in the name field for the record)
With the first record, make sure the Alias option is set to yes, and look for your static site endpoint under the options
Repeat this process for the www domain, this time making sure that the name for the A record includes the www

Steps and Hands On :


Configuring Amazon S3 Buckets to Host a Static Website with a Custom Domain
Introduction
In this live AWS environment, we will create and configure a simple static website. We will go through configuring that static website with a custom domain, using Route 53 Alias record sets. This will demonstrate how to create very cost-efficient website hosting for sites that consist of files like HTML, CSS, JavaScript, fonts, and images.

Code for the static site can be found here.

Solution
Log in to the live AWS environment using the credentials provided. Make sure you're in the N. Virginia (us-east-1) region throughout the lab.

Creating an S3 Static Site and Route 53 DNS Records
Now that we are logged in to the AWS Console, let's bring up the S3 and Route 53 Management Console in new tabs on the browser. These services can be found in the Services dropdown menu on the top left-hand side of the AWS console.
Download the static HTML files from the Git repo provided in the Additional Resources and Information section by navigating to https://github.com/linuxacademy/csa-a-2018 in a new tab in our browser.
Click the green Clone or download button.
Click Download ZIP.
Creating an S3 Bucket
Our S3 bucket will need to be named the same as our Route 53 hosted zone that was created as part of this hands-on lab. If you are following along with the video, please note that the name listed may be different for every lab.
We can find this name by navigating to the Route 53 Management Console tab we opened earlier, clicking on Hosted zones under the DNS management section, and then copying the domain name listed here (e.g., cmcloudlab157.info.).

Now that we have our static HTML files and have copied the Route 53 hosted zone name, let's create an S3 bucket.

In the S3 Mangement Console tab that we opened earlier, click the Create bucket button.

Within the Create bucket window, we will need to provide some information for our new bucket, so we will paste the Route 53 hosted zone name into the Bucket name field.

Note: Be sure to remove the trailing . at the end of the hosted zone domain name.

With the bucket name provided, we can click the Next button to continue.

The Configure options page has several options that we can configure for our bucket. However, this lab focusses on creating an S3 bucket for a static site, and we will not make use of these settings at this time.

Click the Next button to continue.
Click Next on the Set Permissions window section.
Under Manage public permissions dropdown box change it to Grant public read access to this bucket.
Click Next.
Click Create bucket.
Upload Files to an S3 Bucket
Now that our bucket has been created, it will show in the list in our S3 service tab. To see more information about this bucket, let's click the name of the bucket in the list to take us to the Overview tab of the bucket.
From this page, we can upload all of the static HTML files we downloaded earlier from the Git repo. We will need to extract the ZIP file we downloaded earlier before we can continue.

Note: This process is different for each operating system.

Once we have the ZIP file extracted, click the blue Upload button on our S3 bucket's Overview tab.
Inside the Upload popup, we can click the Add files button to select which files we would like to upload.
Navigate to the extracted folder named csa-a-2018-master, and then open the penguinsite folder.
Select the four HTML files inside of this folder, and then select Open. Our four HTML files should now be listed in the Select files section of the Upload window.
Let's continue by clicking the Next button.
We will now need to set the permissions for the files that we are uploading.

In the Set permissions section, select the first dropdown menu under the Manage public permissions section and click Grant public read access to this object(s).

Note: This will allow everyone read access to these files, which is a requirement for our static site.

We can now continue by clicking the Next button.
We will not need to change any settings on the Set properties tab, so we can continue by clicking the Next button.
We Can review the information on the Review tab, and then click the Upload button to begin uploading our files. Once the files are uploaded, they are listed in the Overview tab of our S3 bucket.
Configure Files in an S3 Bucket for a Static Site
Now that our files are uploaded to our S3 bucket, we will need to configure the index.html and error.html files for use with our static site.

Click on the Properties tab.
Scroll down and click Static website hosting to expand the configuration options for this section.
With the configuration options expanded, check the button for the option Use this bucket to host a website, which will give us the option to fill in the Index document and Error document fields.

Note: These fields suggest the names of the files to enter, but we will still need to type the names of the files in each field.

For this lab, the Index document field should be filled in with index.html and the Error document should be filled in with error.html.
Once the fields are filled, click the Save button to continue.
We can now view the site by clicking the Static website hosting section again and then opening the Endpoint link provided at the top of this section in a new tab. This opens our static site that is hosted on AWS's S3 website endpoint, as indicated by s3-website-us-east-1 in the URL for the site.

Note: Normally, we would create a custom domain name to point to a specific IP for the server that is hosting the website, but since the IP for the S3 website endpoint will change, we will need to do something a little different.

Creating a Route 53 DNS Record
Let's switch over to the Route 53 Management Console services tab we opened at the beginning of this lab. From here, be sure that the only record sets that currently exist are one NS record and one start of authority SOA record.

Note: We will need to give AWS about 5-10 more seconds to ensure the configuration options we applied earlier take effect, then it's a good idea to refresh the entire page to ensure we can create our new record set successfully.

After we have given AWS a moment and refreshed the page, click on the Create Record Set button. This will open the Create Record Set section on the right of our page.

Leave the Name field blank for this record set, ensure the Type is set to A - IPv4 address.
Click on Yes for the Alias.

This will give us a new field, Alias Target. When we expand this dropdown, we should see a target listed under the S3 website endpoints section. If you still see No targets available, wait a few more seconds and refresh the page again.

Once our target is there, click on it to populate the Alias Target field with that target.

Click Create at the bottom of this section to create the new A record.

Note: Sometimes it may take up to 15-20 minutes for the new DNS record to take effect.

With the record created, we should now be able to copy and paste the Name of our new A record into a new browser window to navigate to our static site, being sure to leave off the trailing . (e.g., cmcloudlab157.info).

Configuring a www Redirect and Route 53 A Record for the S3 Static Site
Now that our non-www record is created, we need to create a www record to redirect to the non-www record. To do this, let's switch back to our S3 Management Console tab, and then create another bucket using the www version of our domain name.
On the S3 Management Console service page, click on Amazon S3 to bring us back to the service page.
Click Create bucket and then provide our bucket name again, this time entering www. in front of our bucket name (e.g., www.cmcloudlab157.info).
Continue clicking Next until we get to the Set permissions tab.
We need must uncheck the checkboxes for Block new public ACLs and uploading public objects (Recommended) and Remove public access granted through public ACLs again.
Click Next.
Click Create bucket.
Now that our www S3 bucket is created, click on the name of our www S3 bucket, and then navigate to the Properties tab.
Click the Static website hosting section.
Click the radio button for selecting Redirect requests.
We will need to provide the name of our non-www S3 bucket for the Target bucket or domain field (e.g., cmcloudlab157.info), and then click the Save button to apply this configuration option.
We can test that this works correctly by expanding the Static website hosting section again and opening the endpoint link in a new tab on our browser.

Note: This should redirect us to the non-www version for our site (e.g., cmcloudlab157.info, or the name of your domain, will be shown as the URL in the browser).

With the www S3 bucket now redirecting requests to our non-www S3 bucket, we need to add another A record in Route 53. Let's switch back to the Route 53 Management Console service tab.

As before, we will need to give AWS some time to propagate the changes we have made and then possibly refresh the Route 53 Management Console page.
Once we have applied the changes, we should be able to click Create Record Set.
Type www into the name field.
Click Yes for Alias, and we should see our www target listed under S3 website endpoints for our Alias Target.
Click on that target to populate the field with that target name.
Click Create to save our record.

Note: Just as before, it may take some time for the new A record to take effect. Give AWS some time to propagate the DNS change, and then we should be able to navigate to our site using the www name in our browser (e.g., www.cmcloudlab157.info).

Testing with dig and nslookup
While we are waiting on our DNS records to propagate, we can learn a little about dig and nslookup and how they can be used to troubleshoot DNS. In a terminal window (on either your local machine or a Cloud Playground server), let's run the command dig <DOMAIN NAME>, replacing <DOMAIN NAME> with the name of the domain provided to you in this hands-on lab.

If our DNS records have not propagated yet, we will see output that is something like this:

$ dig cmcloudlab157.info

; <<>> DiG 9.10.6 <<>> cmcloudlab157.info
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30479
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;cmcloudlab157.info.        IN    A

;; AUTHORITY SECTION:
cmcloudlab157.info.    213    IN    SOA    ns-439.awsdns-54.com. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400

;; Query time: 3 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Mon Feb 25 10:48:18 EST 2019
;; MSG SIZE  rcvd: 129
The only records returned under the AUTHORITY SECTION is an SOA record and not the A records that we created, so we can infer that the created records have not propagated yet.

We can also check this using nslookup by issuing the command nslookup <DOMAIN NAME>, again replacing <DOMAIN NAME> with the name of the domain provided to you in this hands-on lab.

If our DNS records have not propagated yet, we will see output that is something like this:

$ nslookup cmcloudlab157.info
Server:        192.168.1.1
Address:    192.168.1.1#53

Non-authoritative answer:
*** Can't find cmcloudlab157.info: No answer
The Non-authoritative answer section can't find the domain name, so we can infer that the A records we created have not propagated yet.

After we have waited for the DNS records to propagate, we can run these commands again and see much more output than before. For dig, our output will now look something like this:

$ dig cmcloudlab157.info

; <<>> DiG 9.10.6 <<>> cmcloudlab157.info
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61903
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 4, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;cmcloudlab157.info.        IN    A

;; ANSWER SECTION:
cmcloudlab157.info.    5    IN    A    52.216.238.26

;; AUTHORITY SECTION:
cmcloudlab157.info.    172152    IN    NS    ns-1359.awsdns-41.org.
cmcloudlab157.info.    172152    IN    NS    ns-1779.awsdns-30.co.uk.
cmcloudlab157.info.    172152    IN    NS    ns-439.awsdns-54.com.
cmcloudlab157.info.    172152    IN    NS    ns-536.awsdns-03.net.

;; Query time: 1628 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Mon Feb 25 10:52:58 EST 2019
;; MSG SIZE  rcvd: 204
Note: Notice that the A record we created now shows an IP address for the website endpoint in the ANSWER SECTION and our AUTHORITY SECTION now shows the name servers used to handle DNS for our domain name.

For nslookup, our output should now show something like this:

$ nslookup cmcloudlab157.info
Server:        192.168.1.1
Address:    192.168.1.1#53

Non-authoritative answer:
Name:    cmcloudlab157.info
Address: 54.231.81.209
Note: There is now an address provided in the Non-authoritative answer section, so our A record has propagated successfully.

Conclusion
In this live AWS environment, we have created and configured a simple static website. We have gone through configuring that static website with a custom domain, using Route 53 alias record sets. This has demonstrated how to create very cost-efficient website hosting for sites that consist of files like HTML, CSS, JavaScript, fonts, and images.
