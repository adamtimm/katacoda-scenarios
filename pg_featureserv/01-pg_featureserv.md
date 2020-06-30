This exercise will show you the steps to take to add pg_featureserv to your PostGIS implementation. 

First, take a look at the tab to the right called "pg_featureserv". You'll see that it's still waiting for an available connection on port 9000, the port that pg_featureserv serves data on. That's because we haven't added pg_featureserv to our PostGIS implementation yet. Let's do that now.

**Click Back to ```Terminal```**

To add pg_featureserv to your PostGIS database, you need to either download the [source code](https://github.com/CrunchyData/pg_featureserv), download the binaries, or one of our supported containers. We've already pre-staged the container of pg_featureserv for this scenario.

To add the container to your postgis implementation, you'll need the connection info and username and password (from the first screen). 

The black box below allows you to click on it to have the code execute in the right hand terminal. Be sure to click on the ```Terminal``` tab before click on the box to make sure the code executes in the correct tab. You also have the option of copying and pasting the code, or typing it yourself in the ```Terminal``` tab.

```docker run -p 9000:9000 --env=DATABASE_URL=postgres://groot:password@172.18.0.2/nyc timmam/pg_featureserv:Katacoda```{{execute}}

Now, if you look at the pg_featureserv tab again, you'll see that it has scucessfully connected to the default UI. We'll go over some additional information about how pg_featureserv works and the basic layout of the UI on the next pages.