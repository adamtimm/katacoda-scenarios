This exercise will show you the steps to take to add pg_tileserv to your PostGIS implementation. 

First, take a look at the tab to the right called "pg_tileserv". You'll see that it's still waiting for an available conneciton on port 7800, the port that pg_tileserv serves data on. That's because we haven't added pg_tileserv to our PostGIS implementation yet. Let's do that now.

To add pg_tileserv to your PostGIS database, you need to either download the [source code](https://github.com/CrunchyData/pg_tileserv), download the binaries, or one of our supported containers. We've already pre-staged the container of pg_tileserv for this scenario. 

To add the container to your postgis implentation, you'll need the connection info and username and password (from the first screen). 

```docker run -p 7800:7800 --env=DATABASE_URL=postgres://groot:password@172.18.0.2/nyc timmam/pg_tileserv:Katacoda```{{execute}}

Now, if you look at the pg_tileserv tab again, you'll see the default UI and all of the nyc data being delivered as vector tiles. If you click on the preview link, you can see the vector tiles. You can also click on any of the vector tiles and see all of the attribute information contained in the database. 