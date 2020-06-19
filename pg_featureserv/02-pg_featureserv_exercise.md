This exercise will show you the steps to take to add pg_featureserv to your PostGIS implementation. 

First, take a look at the tab to the right called "pg_featureserv". You'll see that it's still waiting for an available connection on port 9000, the port that pg_featureserv serves data on. That's because we haven't added pg_featureserv to our PostGIS implementation yet. Let's do that now.

To add pg_featureserv to your PostGIS database, you need to either download the [source code](https://github.com/CrunchyData/pg_featureserv), download the binaries, or one of our supported containers. We've already pre-staged the container of pg_featureserv for this scenario. 

To add the container to your postgis implentation, you'll need the connection info and username and password (from the first screen). 

```docker run -p 9000:9000 --env=DATABASE_URL=postgres://groot:password@172.18.0.2/nyc timmam/pg_featureserv:Katacoda```{{execute}}

Now, if you look at the pg_featureserv tab again, you'll see the default UI and all of the nyc data being delivered as features. If you click on the preview link, you can see the features. You can also click on any of the features and see all of the attribute information contained in the database. 