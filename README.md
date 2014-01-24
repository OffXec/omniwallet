## What is Mastercoin?

[from wiki.mastercoin.org]
Mastercoin is both a new type of currency (MSC) and a platform. It is a new protocol layer 
running on top of bitcoin like HTTP runs on top of TCP/IP. Its purpose is to build upon the 
core Bitcoin protocol and add new advanced features, with a focus on a straight-forward and 
easy to understand implementation which allows for analysis and its rapid development. 

## What is this Web-Wallet?

We anticipate most users will not want to understand the technical details of running a standalone wallet.
To counter user dissatisfaction and garner a strong following, Mastercoin will require an easy
to use wallet not requiring user installation. Web-wallets are the typical manifestation of this
requirement, but we recognize the concern for security is not typically heeded. The concern for
security is another motivation in this regard, and a section will be written regarding those 
concerns.

### Risks 

Web wallet security
Traditionally, web wallets are designed with the security implication of handling user data. 
In our case, user data includes bitcoin private keys. Since this is a appealing target for
criminals, our aim in this project will be to minimize the risk of the user in using our 
service, by developing software to run without the handling of user private keys. This has been
attempted with success by the web wallet and blockchain query service blockchain.info. We are 
confident this level of security can and should be implemented by our software.

## Setup

update ~/.sx.cfg with an obelisk server details.  Don't have one already set up?  Here's how to build one on Rackspace: https://gist.github.com/curtislacy/8424181
```
# ~/.sx.cfg Sample file.
service = "tcp://162.243.29.201:9091"
```
Make sure you have python libraries installed.
```
sudo apt-get install python-pip
sudo pip install ecdsa
sudo pip install pycoin
```
Install nginx, and drop in the config included with this codebase.
```
sudo apt-get install uwsgi uwsgi-plugin-python
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update 
apt-get install nginx
exit
sudo cp etc/nginx/sites-available/default /etc/nginx/sites-available
```
Find this section near the beginning of /etc/nginx/sites-available/default:
```
        ## Set this to reflect the location of the www directory within the msc-webwallet repo.
        root /home/cmlacy/msc-webwallet/www/;
        index index.html index.htm;
```
Change the ``root`` directive to reflect the location of your msc-webwallet codebase (actually the www directory within that codebase).
Run npm install
```
npm install
```

## Running

Start nginx by running:
```
sudo service nginx start
```
Using the config included, nginx will launch an HTTP server on port 80.
Start the blockchain parser and python services by running:

```
app.sh
```

This will create a parsing & validation work area in /tmp/msc-webwallet, and begin parsing the blockchain using the server listed in your .sx.cfg file (see above).

## Development

Most of the HTML in /www is checked in, however three files are generated at install:
```
www/Address.html
www/index.html
www/simplesend.html
```
These are generated by the gen_www.sh script as a part of "npm install".  If you need to regenerate them, and do not wish to run a full "npm install" (though that's pretty fast), you can also run:
```
grunt build
```
