# POC-3
### Launch WordPress multisite instance.
### Configure three different websites, i.e. wpprod.domain1.com, domain1.com, and domain2.com.
### Apply SSL on all three websites.
### Configure SSL auto-renewal script.
### Redirect any one domain to an s3 static website page through the webserver conf file.

## Step 1 - Creating the Multisite Instance 

Go to the AWS console -> search for Lightsail Services -> Click on create Instance

![image](https://user-images.githubusercontent.com/67600604/173495128-18bc56ad-e986-4148-81de-714eb5c8897d.png)

Then, click on the Wordpress Multisite with the Linux/Unix
To, create multiple website in the same Instance we are creating the Multisite Instance, so we can deploy more than one domain on single Instance. In our case , I will be purchasing the three domains from freenom.com i.e. page1.ml, page5.ml, page7.ml

Out of these three domains, page1.ml will be the main domain

![image](https://user-images.githubusercontent.com/67600604/173499123-97bbb293-2d3c-4c67-a769-9d0aa70dd45e.png)

After creating, the Instance 

## Step 2 - Configure three different websites, i.e. page1.ml, page6.ml, and page7.ml

Go to the Lightsail Networking -> and make the Instance IP public so that we can mention th static IP to three Domains in domain registerer that is freenom.com and Attach it to the Instance we have created 

![image](https://user-images.githubusercontent.com/67600604/173495980-e151797f-3221-4a12-91ee-59a33ef35ca9.png)

Now, browse the IP/wp-admin to the Browser and you will be able to see a wordpress login console

now generally we have the username as user and to get the password , connect the SSH of the lightsail instance and type the 

### cat $HOME /bitnami_application_password 
and you will get the password to login 

go to the Dashboard -> sites -> add new 

add the wesites domain that you have purchased from the freenom 

![image](https://user-images.githubusercontent.com/67600604/173496667-41fcc52c-b4bb-43f4-8c4f-b34d0d03ed2f.png)

add the static IP to the DNS to connect 

![image](https://user-images.githubusercontent.com/67600604/173496813-94bb8d98-4dc1-47ad-86fc-d9bba250111f.png)

Now, we can check all the three Domains on the Browser and you will see that they are only using the http, not the https

so , now we will create the SSL for the domains 

## Step 3 - Apply SSL on all three website

to create the certificate, connect the instance using the SSH and type 
### sudo /opt/bitnami/bncert-tool

and it will ask you for the domain list, mention your domains seperated by the comma and it will ask that if you want it for www as well? 
## Press Y

![Screenshot (242)](https://user-images.githubusercontent.com/67600604/173498599-2555d7d8-5226-4603-ae05-d8d80693c360.png)

![Screenshot (243)](https://user-images.githubusercontent.com/67600604/173498813-fd423a07-6e57-4dfd-b8ea-8593fc1b440d.png)

Now, visit the three wesites , they are working on the https.

![Screenshot (240)](https://user-images.githubusercontent.com/67600604/173498889-8f314817-ace2-4a6a-9136-45278596da3c.png)
![Screenshot (238)](https://user-images.githubusercontent.com/67600604/173499153-43174154-1b12-47ad-8a8b-bcabaa15c8f2.png)
![Screenshot (239)](https://user-images.githubusercontent.com/67600604/173499169-4ecabb00-8ad8-4b56-931c-f660e2804655.png)

All the three wesites are now secure.

Now the last step is 
## Step 4 - Redirect any one domain to an s3 static website page

### Prerequisite is to have a other domain at which you have to redirect 

We need to create bucket with name “page7.ml”. we are redirecting the page7.ml at shruti2.tk 

![image](https://user-images.githubusercontent.com/67600604/173499962-9d61c598-9a90-43d3-8d3d-5707f0e1608d.png)

allow all the public access to the bucket 
We need to step site redirection using “Static web hosting” feature of S3
![image](https://user-images.githubusercontent.com/67600604/173499588-322b9f50-6572-41b4-9c86-b5882e135cad.png)

and click on the Redirectin and mention you 2nd domain here. in our case, it is shruti2.tk

now paste the above link as CNAME in your DNS registerer, in our case it is , Route53 to redirect and click on the alias and connect the bucket that we have created
"page7.ml"

![image](https://user-images.githubusercontent.com/67600604/173500114-5c234123-fb78-47f5-822d-3251f1ea48e2.png)

Now, test the website is redirecting or not. If it is redirecting then it will show the 
### "301 Moved Permanently"

![image](https://user-images.githubusercontent.com/67600604/173500296-602de65c-7c7c-4ca9-813c-2c25c34e4578.png)
![image](https://user-images.githubusercontent.com/67600604/173500402-e69e85a4-0f80-4916-90cf-145b481494d3.png)

To renew the Certificate from let's encrypt

Let’s Encrypt uses the client Certbot to install, manage, and automatically renew the certificates they provide

### sudo certbot renew

If you have multiple certificates for different domains and you want to renew a specific certificate, use

### certbot certonly --force-renew -d page1.ml page7.ml page6.ml

The --force-renew flag tells Certbot to request a new certificate with the same domains as an existing certificate. The -d flag allows you renew certificates for multiple specific domains

To verify that the certificate renewed, run:

### sudo certbot renew --dry-run

If the command returns no errors, the renewal was successfu



SUCCESFULL 
