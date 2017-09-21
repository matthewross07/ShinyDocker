---
title: "Shiny Stevedore Setup"
output: html_document
---

##Intro to Shiny and Stevedore

Shiny R is a simple and powerful way to make and share interactive data visualizations. Shiny R can be used for sharing interesting data analyses in an interactive way like this dialect quiz and [map](http://www.nytimes.com/interactive/2013/12/20/sunday-review/dialect-quiz-map.html?_r=0), or as a way to test parameters in a complex [model](https://ahalterman.shinyapps.io/BayesCalculator/). For more on what shiny is and how it works you can go to RStudio's [webpage](https://shiny.rstudio.com/). These apps can vary in data storage and processing demand. For simple Apps RStudio allows any user to upload up to 5 free applications that are less than 2 gb of data storage and less than 512 mb of RAM, with limited hours of use per month. If you have an application that you think might need more memory, more RAM, or more hours of use, then Duke's Shiny Stevedore support is an excellent way to host your app in a simple and easy way. 

Stevedore uses Docker to create miniature web-servers that can be scaled to what the user needs in terms of memory or processing power. These are exceptionally low-cost servers compared to using a regular server setup and they are simple to use, requiring a shiny app to be stored on a public git repository (like GitHub or GitLab). Guidance on setting up a shiny app or github repository is beyond the scope of this help document, bu there are ample resources online to get started. 

##Setting up your repository

###Simple app
For this example, we will use two GitHub repositories as examples. The first one is a simple shiny app with a shiny server script only ( server.r, ui.r, with data prep and data files). This website is located on a public github repository [here](https://github.com/matthewross07/WR440.Lesson) and was made using RStudio's git support. For help learning how to get shiny apps pushed to github, Jenny Bryan has an extremely helpful [webpage](http://happygitwithr.com/). Once you have your shiny app repository hosted on a public github repository, you can spin up your app using stevedore. Before you do this, you should make sure your app is working locally and in various web-browser, because once the App is spun up it will be publically viewable. 

###Multiple apps, single docker container
If you are using multiple apps in a single docker container you will need to write an index.html file that links to all of those apps. An example multi-app repository can be found [here](https://github.com/matthewross07/IntroENVShiny). 

###Additional R or Linux packages
The core server is setup with many of R's most popular packages, but most likely you will need to install additional r packages and potentially even stevedore packages. To do so is quite simple. First you need to make a folder labeled exactly: ".stevedore" and in that package you can make two text files. The first labeled "r_packages" should contain space separated R packages by name as shown [here](https://github.com/matthewross07/IntroENVShiny/blob/master/.stevedore/r_packages). The second labeld "ubuntu_packages" should contain space separated ubuntu packages like "gdal-bin".  

####R Packages included:
'rmarkdown','devtools','xts','ggvis','dygraphs','tidyr','dplyr',
'lubridate','ggplot2','shiny','rgdal','sp','raster','rasterVis',
'reshape2','shape','maptools','fields','magicaxis','leaflet'"

####Linux packages included
'gdebi-core', and 'gdal-bin libgdal1-dev libproj-dev'

##Starting app using stevedore
Now that your repository is ready we can host it!  Stevedore is the web hosting environment where you will host your app.

1. To access stevedore go to:"https://stevedore.oit.duke.edu/" where you will be asked to login with your Duke Shibboleth account. 

2. Once you are logged in you will need to click a button for "Create a New Service."

3. This will open a new page with a form to fill out.
  * Application - select "Shiny"
  * URL - choose a URL that must end in ".web.duke.edu", something like "shiny.web.duke.edu" or "application.web.duke.edu" will work. 
  * Fund Code - Enter in a fund code for the project (pricing can be found through Duke OIT)
  * Service Level - Decide if you want 24/7 support for your app. 
  * Network - Choose public if you want this to be publically viewed choose private if you only want Duke users (with shibboleth to use your app)
  * Git Repository - Now you have to point stevedore to your github directory with your shiny app. This address must end in ".git" For the apps above these links would be: 
    * https://github.com/matthewross07/WR440.Lesson.git 

    * https://github.com/matthewross07/IntroENVShiny.git
    
4. Now your app will start building! Normally this only takes a few minutes, but the URL will first have to be verified by someone at OIT which can take a few hours or days. Contacting OIT before you submit your request for a shiny app will make this process faster. 

##Troubleshooting
Inevitably your app will not work or will breakdown. Most often this is becasue a package is not on the server or has been updated and certain syntax you use in your code no longer works. Luckily updating an app is very simple!

1. First check that the app works locally again and see if you need to update any code, do so on your local machine and push the changes to github.

2. Second, if the app webpage is broken you can right click on the webpage and click "inspect" this will open a part of your browser where you can see any errors that your server is throwing. Make sure to click on the "console" to see if it is shiny errors. This can often highlight missing packages. Use the above directions to add more packages to your r_packages or ubuntu_packages as needed. 

3. Finally, you can directly investigate server issues by navigating to your app on stevedore.oit.duke.edu. Once inside the app you will see a header: "Instance Containers" and underneath that there will be a table with  a row for the Shiny Server and the Apache Server. You can click on the cells under the column labeled "ID" which will be an alphanumeric string. Once you click on the Shiny Server ID you will see a column "Console Output" you can click on this and you should see some that the server setup is done. If not you can have an array of errors that will highligh missing packages or broken code. If this is working but there are errors in the apache server side of things then you can contact OIT for help. 

4. Once you have fixed the issue with your app, you can click navigate back to the main landing page for your app on stevedore. Under the Instances tab, there is a button for "rebuild containers," this will destroy the current instance and grab your app from your github directory again, completely rebuilding your app from scratch. Hopefully this fixes the problem and your app is up and running beautifully. 

#Good luck and enjoy shiny, stevedore, and docker. 


