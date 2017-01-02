### Installing Ghost
Assuming you've already read [part 1](https://blog.zsec.uk/quick-server-p1/). Now that we have a somewhat setup server with required settings and the like, it's time to install ghost blog, for this here is a short all in one script for ease of setup, does it all in one:

			echo ""
			
			######Check to make sure script is being run as root######
			if [ `whoami` != root ]; then
				echo "This script must be run as root"
				exit 1
			fi
			
			echo "This script installs Nginx and Ghost on Debian"
			echo "To configure Nginx and Ghost please provide your hostname:"
			echo ""
			read HOSTNAME
			
			######Download and install Node######
			apt-get -y update
			apt-get -y upgrade
			curl -sL https://deb.nodesource.com/setup | sudo bash -
			apt-get install -y nodejs zip nginx ccze sudo
			
			######Download and install Ghost######
			mkdir -p /var/www
			cd /var/www/
			curl -L -O https://ghost.org/zip/ghost-latest.zip
			unzip -d ghost ghost-latest.zip
			rm ghost-latest.zip
			cd ghost/
			sed -e "s/my-ghost-blog.com/$HOSTNAME/" <config.example.js >config.js
			/usr/bin/npm install --production
			
			#######Setup Ghost User######
			adduser --shell /bin/bash --gecos 'Ghost application' ghost --disabled-password
			echo ghost:ghost | chpasswd
			chown -R ghost:ghost /var/www/ghost/
			
			######Config Nginx######
			echo "configuring Nginx"
			echo "server {" >> /etc/nginx/sites-available/ghost
			echo "    listen 80;" >> /etc/nginx/sites-available/ghost
			echo "    server_name $HOSTNAME;" >> /etc/nginx/sites-available/ghost
			echo "    location / {" >> /etc/nginx/sites-available/ghost
			echo "        proxy_set_header   X-Real-IP \$remote_addr;" >> /etc/nginx/sites-available/ghost
			echo "        proxy_set_header   Host      \$http_host;" >> /etc/nginx/sites-available/ghost
			echo "        proxy_pass         http://127.0.0.1:2368;" >> /etc/nginx/sites-available/ghost
			echo "        }" >> /etc/nginx/sites-available/ghost
			echo "    }" >> /etc/nginx/sites-available/ghost
			
			ln -s /etc/nginx/sites-available/ghost /etc/nginx/sites-enabled/ghost
			
			rm /etc/nginx/sites-available/default
			rm /etc/nginx/sites-enabled/default
			service nginx restart
			
			######Install PM2######
			echo "#!/bin/bash" >> /home/ghost/start.sh
			echo "export NODE_ENV=production" >> /home/ghost/start.sh
			echo "cd /var/www/ghost/" >> /home/ghost/start.sh
			echo "npm start --production" >> /home/ghost/start.sh
			chmod +x /home/ghost/start.sh
			
			/usr/bin/npm install -g pm2
			
			su -c "echo 'export NODE_ENV=production' >> ~/.profile" -s /bin/bash ghost
			su -c "source ~/.profile" -s /bin/bash ghost
			su -c "/usr/bin/pm2 kill" -s /bin/bash ghost
			su -c "env /usr/bin/pm2 start /home/ghost/start.sh --interpreter=bash --name ghost" -s /bin/bash ghost
			env PATH=$PATH:/usr/bin pm2 startup debian -u ghost --hp /home/ghost
			su -c "pm2 save" -s /bin/bash ghost

####Setting up Mail on Ghost
Once this script has been run, it is highly recommended that you setup smtp with ghost for password resets, (SUPER LIFE SAVER). To do this follow the steps below:

 1. Create an account on  [mailgun](http://www.mailgun.com) (it's free)
 2. Once you've signed up and activated your accrount navigate to mailgun account>domains>click on your domain name and take note of `Default SMTP Login` & `Default Password`
 3. Navigate to your server where ghost is installed, Open up config.js in your favorite command line editor and Put your ‘Login’ from mailgun between the quote marks next to ‘user’ and your ‘Password’ from mailgun inside the quotes next to ‘pass’.


     `mail: {
    transport: 'SMTP',
        options: {
            service: 'Mailgun',
            auth: {
                user: 'YOUR DEFAULT SMTP Login',
                pass: 'YOUR PASSWORD'
            }
        }
    }`

 If I was configuring mailgun for the ‘zephrfish’ account, it would look like this:
 

   ` mail: {
        transport: 'SMTP',
        options: {
            service: 'Mailgun',
            auth: {
                user: 'postmaster@zephrfish.mailgun.org',
                pass: 'lolub3rs3cur3p4ssw0rd'
            }
        }
    }`


Keep an eye out for all of the colons, quotes and curly brackets. Misplace one of those and you’ll find you get weird errors.

You can reuse your settings for both your development and production environment if you have both.

When you have finished making changes to the config.js file don’t forget to run
 `pm2 restart ghost` to restart ghost.

When this is done you should be all setup with a server running ghost blog platform, poke me on twitter if you need a hand more [ZephrFish](https://twitter.com/ZephrFish).
