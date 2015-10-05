Setup Ghost for GitHub Pages
============================

To build your own static GitHub Pages with Ghost, just follow these steps.

Install Node.js
---------------

Ghost is written in Node.js, so you will need the Node.js runtime.

    brew install node  
    brew install wget  
    
Download and Install Ghost
--------------------------

Check for the latest version of Ghost from Ghost.org

    mkdir ghost  
    cd ghost  
    wget --no-check-certificate https://ghost.org/zip/ghost-0.5.2.zip  
    unzip ghost-0.5.2.zip  
    npm install --production  
    
Start Ghost
-----------

    npm start  
    
Login to Ghost
--------------

    open http://localhost:2368/ghost
    
Create an account, read the intial blog post how to edit with Markdown etc.

Create your GitHub Pages repo
-----------------------------

See the [GitHub Pages Basics Documentation](https://help.github.com/categories/20/articles) for details.

You must use the `username/username.github.io` naming scheme. The repo name must be **lower case** even if your username has upper case letters.

Install Buster
--------------

With the tool [Buster](https://github.com/axitkhurana/buster) you can export the Ghost blogs into static pages. First we install Buster with:

    brew install python  
    sudo pip install buster  
    
Prepare static folder
---------------------

I have placed the `static` folder for Buster inside my Ghost installation. So I just cloned my `username/username.github.io` repo with a target directory name like this:

    git clone git@github.com:username/username.github.io.git static
    
Write Blog posts
----------------

Now your setup is complete and you can start writing Blog posts in Ghost.

Export Ghost with Buster
------------------------

All published blog posts could be exported to the `static` folder with Buster.

    buster generate --domain=http://localhost:2368  
    buster preview  
    open http://localhost:9000
    
The output of Buster could be previewed on port 9000.

Deploy to GitHub Pages
----------------------

    buster deploy  
    
This will add, commit and push all files in the `static` folder to your GitHub repo.

For the initial push, please wait up to 10 minutes until GitHub deploys your subdomain.

All upcoming pushes are much faster and you can see your static Ghost blog posts on https://username.github.io

Write new Blog posts
--------------------

To write new Blog posts or update the existing ones, just start Ghost, edit, then generate and deploy the static pages.

    cd ghost  
    npm start  
    buster deploy  
    
The sed commands will fix the share links from localhost to your correct GitHub pages URL.

That's it.

Fixing links
------------

**Update** It seems that buster has problems fixing all hyperlinks. So I made this script that works for me:

    #!/bin/bash
    buster generate --domain=http://127.0.0.1:2368  
    find static/* -name *.html -type f -exec sed -i '' 's#u=https://stefanscherer.github.io#u=https://stefanscherer.github.io#g' {} \;  
    find static/* -name *.html -type f -exec sed -i '' 's#url=https://stefanscherer.github.io#url=https://stefanscherer.github.io#g' {} \;  
    find static/* -name *.html -type f -exec sed -i '' 's#href="https://stefanscherer.github.io#href="https://stefanscherer.github.io#g' {} \;  
    find static/* -name *.html -type f -exec sed -i '' 's#src="https://stefanscherer.github.io#src="https://stefanscherer.github.io#g' {} \;  
    find static/* -name *.html -type f -exec sed -i '' 's#link>http://localhost:2368#link>https://stefanscherer.github.io#g' {} \;  
    buster deploy  
    
This will generate the new pages, fix all links and then deploy them on GitHub.