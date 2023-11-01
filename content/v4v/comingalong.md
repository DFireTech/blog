---
title: "It's All According To Plan (but not mine)"
date: 2023-08-22T19:08:18-05:00
draft: false
tags: ["Bitcoin", "V4V", "Blogging", "LN"]
---
*It seems that the tooling was there, I am just not well versed in Google Fu.*

# The Value Train is Coming!

Welcome one and all! Value for Value is here in blog land and it was *right before our eyes.*
It seems the tooling was there, I am just not well versed in Google-Fu! 
[BeardedTek](https://beardedtek.org/get-a-boost-with-alby/) has figured out how to get boosts working (WITH A MODAL SCREEN!!!) with [Alby](https://getalby.com). This is very exciting, as it seems that with this same tooling, I can get the BTCPayServer going with this modal button as well. 

## There Are Others Like It, But This One Is Mine

Initially, BeardedTek had found [Mash](https://app.mash.com/earn) which looks *great* at first glance. As Andreas Spies says, "But, we want more". 
The issue with Mash is that they inject trackers, google ad stuff, etc into your page, which for us more privacy minded folks is a no go. I agree, so, lets look into what BT has done!

## Boooooooost!
> You know that PayPal donation you make to your favorite software project or creator? PayPal takes away nearly 3% + 0.49$! With Lightning payments, it's roughly .04$ per transaction, or $46 more coming in over 100 $1 transactions! -BeardedTek
You read that correctly. If you make a PayPal dono at a dollar, you can expect about half of that to get routed for fees to them. Seems *very bad*, right?
I don't expect any of you to just hop on the Bitcoin/Lightning wagon overnight, but, this is extrodinary stuff. Instant payments, extremely low fees, and with comments, its just *Pure Value*
Go read BeardedTek's blog, he did a tremendous job breaking it down for us. 

I, however, already have a BTCPay server set up, so, here's how I did it.

### BTCPay

So, let's get the assumptions/what you will need out of the way *first*.

- Umbrel Node with synchronized Bitcoin Core node
- SSH Access to said node
- Lightning Node
- BTCPayServer App installed on this node
- Domain name/Networking knowledge that I don't have (I used a Sub.Domain setup)
- VPS Access
- Tailscale set up

This guide can be found over at [Orange.Surf](https://orange.surf/public-btcpay-umbrel-tailscale/)

1. Run through the setups of each. BTCPay allows you to connect your LN Node or set up a  wallet internally. I use Zeus Wallet to manage my node and Alby wallet.
2. Go ahead and SSH into that Umbrel node. I missed the .local name of mine, but, when you install Umbrel, it should give you one at the end of the install. `nodename.local` or similar.
3. Using whatever CLI editor you like (Nano roolz), edit the .env.app_proxy file
`{text editor command} ~/umbrel/app-data-btcpay-server/.env.app_proxy`
4. Go ahead and enter `PROXY_TRUST_UPSTREAM=true` in there.
5. Save and exit. Restart BTCPay `~/umbre;/scripts/app restart btcpay-server`

### To the VPS

After you have that all setup, let's hop over to our VPS, like Linode (The guide suggests LunaNode for this as they accept BTC payments already, so, whatever you are comfy with is cool)

With [Linode](https://linode.com), you will (after signing up, of course):

1. Create > Linode
{{<center src="/howtos/createLinode.png" caption="First steps">}}

2. The cheapest option here is a 1GB Nanode that is $5 a month as of writing. The top 4 are here as an example:

{{<center src="/howtos/sharedCPU.png">}}

{{<center src="/howtos/priceToPerform.png" caption="Top Four on the Board">}}

3. I kinda skipped a section here. Just above the type selection you can select your choice of distro and region. 
Select one that makes you comfortable, I go with a Debian or -based distro myself. The region will be specific to you, make one that is close. 

{{<center src="/howtos/distroRegion.png">}}

4. Make the label and set your root password

{{<center src="/howtos/extraStuff.png">}}

5. Let it rip! (there would be an epic Tyson GIF here, but, hugo wouldn't serve it. Boooooo)
Let the box boot up and then lets SSH into it. 

6. From here, switch to root user (if not already) `sudo su` and update
`apt update && apt upgrade -y`

7. Install certbot and nginx
`apt install -y certbot nginx`

8. Lets get Tailscale up and at 'em
`curl -fsSL https://tailscale.com/install.sh | sh`

9. After the `"Installation complete..."` comes up, we will `sudo tailscale up` and copy the authentication URL into your browser. 


### To your Domain Registrar

This is probably the easiest and hardest part if you havent dealt with your registrar before. 

With mine, I had to create a subdomain for this with my domain **THEN** make an A-Record with that to move forward. 
This will all depend on your registrar, but, refer to their documentation if you have never done this before. 

### SSL and Lets Encrypt

Still in your VPS box, lets generate some stuff!
`openssl dhparam -out /etc/ssl/certs/dhparam.pem 4096`
*This could take awhile, so, make a coffee or something*
After that is done, lets make the lets encrypt work out for us
```
mkdir -p /var/lib/letsencrypt/.well-known
chgrp www-data /var/lib/letsencrypt
chmod g+s /var/lib/letsencrypt
```

### Nginx Stuff

We will need a map file to forward our stuff, lets do that.
`{text editor} /etc/nginx/conf.d/map.conf`

In that map, we will have the following:

```
map #http_x_forwarded_proro $proxy_x_forwarded_proto {
	default $http_x_forwarded_proto;
''	$scheme;
}

map $http_upgrade $connection_upgrade {
	default upgrade;
''	close;
}
```

Save and exit the editor

### Back to BTCPay

We will need a config file for nginx to read from for your domain

`sudo nano /etc/nginx/sites-available/btcpayserver.conf`

*Warning* I will be using the same clue-ins as the article above *Warning*


*Warning* {} becomes server_name your.domain.com *Warning*
```
server {
	listen 80;
	server_name {enter your domain here without the braces};

	# Lets Encrypt verify
	location ^~ /.wellknow/acme-challenge/ {
		allow all;
		root /var/lib/letsencrypt/;
		default_type "text/plain";
		try_files $uri =404;
	}
	
	# Redirect everything else to HTTPS
	location / {
		return 301 https://$server_name$request_uri'
	}
}
```

Check that your config is good with `sudo nginx -t`
Create a symlink with `ln -s /etc/nginx/sites-available/btcpayserver.conf /etc/nginx/sites-enabled/`
Reboot NGINX `systemctl restart nginx`
Test again `service nginx configtest`
Reload `service nginx reload`


### Certbot Time!

*Warning* Replace the admin@mydomain and btcpayserver.mydomain.com with your info *Warning*

`certbot certonly --agree-tos --email admin@mydomain.com --webroot -w /var/lib/letsencrypt/ -d btcpayserver.mydomain.com`

You *should* get a Congratulations message here. 


### Back to the btcpayserver conf file
Lets add more stuff into the file, shall we?

```
server {
    listen 443 ssl http2;

    # Replace {btcpayserver.my.domain} with your domain
    server_name {btcpayserver.mydomain.com};
    ssl_certificate /etc/letsencrypt/live/{btcpayserver.mydomain.com}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{btcpayserver.mydomain.com}/privkey.pem;
 
    # Replace {umbrel-tailscale-ip:port} with your details 
    # i.e. http://100.0.0.0:3003
    location / {
      # URL of BTCPay Server 
      proxy_pass {umbrel-tailscale-ip:port};
      proxy_set_header Host $http_host;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Upgrade $http_upgrade;
      proxy_http_version 1.1; # ensure replies through domain
      # For websockets (used by Ledger hardware wallets)
      proxy_set_header Upgrade $http_upgrade;
    }

    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # Disable SSL and old TLS versions
    ssl_protocols TLSv1.2 TLSv1.3;
    
    # Use Diffie-Hellman (DH) key exchange parameters
    ssl_dhparam /etc/ssl/certs/dhparam.pem;

    # intermediate configuration
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;
    
    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;
}
```

Save it and exit. 
From here, go ahead and enter root if you aren't `sudo su` and test the config again as before. `service nginx configtest`
If all is well, reload the service and restart nginx
```
service nginx reload
systemctl restart nginx
```

### Test out your new domain!

From there, you should be able to access the service over your custom domain! How cool, right?

### Getting a pay button

After setting up your "store" as they are called here (you can actually run a store from here, its really cool.), lets get that boost button!
{{<center src="/howtos/btcPayButton.png">}}

You can scroll your pay button page and configure it how you like. First, go ahead and enable the button. 

*Warning* This button should only be used for tips and donations *Warning*

Then you get all of your options like
- Custom Amount
- Use Modal
- Customize Pay Text
- Image Size

Etc. After that, we will need to add this into our site. I did mine as a button, so, I added a shortcode into hugo

### Shortcodes!

*back in timbuktu*

In your `blog` directory, lets navigate to `/layouts/shortcodes`
Dont have that yet?
Nav to the layouts directory and `mkdir shortcodes`

Shortcodes are small snippets of html that we can insert into our site! Neat eh?

Go ahead and create a new file in the terminal
`{text editor} boost.html`
Save it blank and exit.

In the BTCPay server config area for your pay button, after you have configured it, you can copy and paste that snippet in using a GUI editor (probably an easier way, but, hey)
*Make sure to save it*

### In the new blog post

I have actually used some shortcodes in this blogpost! All my photos are centered thanks to a shortcode. 
{{<btcboost>}} 
is a shortcode as well. 
{{<kofi>}}
is also one that I have copied up! This one is from Ko-fi that allows for fiat donos and stuff, but, I think that will be going away for me as well. 

#### Phew, what a blog post

If I have messed anything up, comment it for me and let me know. I will update the post in a jiffy and let you know!

I have to give it up to BeardedTek on this one. He was able to find a service and get this rockin. He uses Alby for his, so his method is great. 
I also use Alby as a backup, so, I will be adding that to my repertoire real soon!  

Eventually, I would love to combine the ease of use of BTCPay and a comments box in one go. That service is GREAT and they have a satsmine on their hands. You can integrate BTC into your shopify so you can be a BTC Biz! If/When I ever get merch for the blog, I will probably go with that for the setup for sure. 

## What Does the Future Hold

In my efforts to cut back, I am looking to re-host my biz store and create a Hugo site or a Ghost blog site,
If I can ever learn Python and Django, the site will be built on that instead. For now, I have a feeling that this is where I will go with it. 


For now, be kind to one another. Make sure to spread some positivity where and when you can. 