Open a terminal on Kali Linux and use the following command.

**sudo apt update**

**sudo apt install nginx**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*CTh91hKD1oXQhDKphb92Lw.png)

After installing, try entering the local IP or localhost. If the installation is successful, a successful display will appear.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*A67v1UJBFYhsatbHK1gZIQ.png)

The results are different from the guide, but that’s okay. My browser showed “Debian Apache2 Default Page”. Let’s dig a little deeper.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*UzaXrRzfoxEgCykLxy0tGw.png)

Now trying to turn on the service.

**sudo systemctl status nginx**

**sudo systemctl enable nginx**

**sudo systemctl start nginx**

If you want to turn off the service, use this command.

**sudo systemctl status nginx**

**sudo systemctl stop nginx**

**sudo systemctl disable nginx**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*-Q4s9CmIRfANZpvqKSoSPA.png)

After activating the service, then opening the browser again, the result is the same. Try entering the curl command below.

**curl 192.168.137.128**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*2q9s5SmTy_oB45Y18BzM5A.png)

It turns out the contents are as usual. Hmm… I don’t understand if this is an error or correct. Let’s take a closer look, let’s go to the Nginx folder.

![](https://miro.medium.com/v2/resize:fit:592/format:webp/1*AyjQOCrHtkD9T685noMKVA.png)

It turns out that there are 2 files, namely, “access.log” and “error.log”. Let’s see the contents.

**cat access.log**

**cat error.log**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*gx5mAppKcrD6aC5OnM21hA.png)

In the first 2 lines I browse nginx, while on the 3rd line, I do the curl command. Here I try to change the URL in the browser to see if there is any change in access.log.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*SzDVPP8-d1mL-XI1NiI1tA.png)

I add “/.git” and then press enter. The result is “404 Not Found” and below it says Nginx and version 1.24.0.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*R-yoaudLSIcGOazcFhifEg.png)

It turns out that if I change the URL, access.log also shows what I changed.

Hmm… interesting! Let’s take a look at the contents of error.log.

![](https://miro.medium.com/v2/resize:fit:570/format:webp/1*ccekg29lgfQgOLKA7dpHkw.png)

It was empty. Let’s move on for now. We will now create our website.

**cd /var/www**

**sudo mkdir tutorial**

**cd tutorial**

**sudo "${EDITOR:-nano}" index.html**

![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*4dP9lt4Jz70E9IWZqBUsxQ.png)

Paste the following to the **index.html** file:

<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello, Nginx!</title>
</head>
<body>
    <h1>Hello, Nginx!</h1>
    <p>We have just configured our Nginx web server on Ubuntu Server!</p>
</body>
</html>


![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*V7GaJU32C5Vc6UGC_OfSIg.png)

To set up a virtual host, we need to create a file in **/etc/nginx/sites-enabled/** directory.

**cd /etc/nginx/sites-enabled**

**sudo "${EDITOR:-nano}" tutorial**
server {
       listen 81;
       listen [::]:81;

       server_name example.ubuntu.com;

       root /var/www/tutorial;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*F-Dt9gdpN8KWFu-LeiSnlA.png)

For this tutorial, we will make our site available on the 81 port, not the standard 80 port. You can change it if you would like to.

root is a directory where we have placed our .html file. index is used to specify files available when visiting the root directory of the site. server_name can be anything you want, because you aren’t pointing it to any real domain by now.

To make our site work, simply restart the Nginx service.

**sudo service nginx restart**

**sudo service nginx status**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Kv_tAoB1uzg_QLsHzsZk6g.png)

Then we go back to the browser and try the local IP or localhost with port 81.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*_jGdo_qbZh1qQ7bbuof7_A.png)

We finally made it. :)

Let’s check the access.log again.
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*ccbqUaczdc3N9PjjSfxl5g.png)

It seems that the Nginx web server is on access.log and using a local IP with port 81 is also possible.

I think that’s enough for now. Regards.





