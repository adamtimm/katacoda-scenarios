This exercise will show you the steps to take to add pg_tileserv to your PostGIS implementation. 

First, take a look at the tab to the right called "pg_tileserv". You'll see that it's still waiting for an available conneciton on port 7800, the port that pg_tileserv serves data on. That's because we haven't added pg_tileserv to our PostGIS implementation yet. Let's do that now.

To use pg_tileserv, you need to have a PostGIS database with spatial data loaded in it. The database has already been started and the spatial data has already been loaded. This scenario will use data from New York City (NYC). This exercise shows how to connect pg_tileserv to your database and shows the resulting vector tiles. We also show an example of using user defined function through pg_tileserv. If you would like to learn more about creating funcitons, please review some of our other exercies. Data from this scenario is used in our other exercises as well, but the environements don't persist between scenarios.

Here are the details on the database we are connecting to:

1. Username: groot
2. Password: password
3. A database named: nyc

To add pg_tileserv to your PostGIS database, you need to either download the [source code](https://github.com/CrunchyData/pg_tileserv), download the binaries, or one of our supported containers. We've already pre-staged the container of pg_tileserv for this scenario. 

To add the container to your postgis implentation, you'll need the connection info and username and password. 

```docker run -p 7800:7800 --env=DATABASE_URL=postgres://groot:password@172.18.0.2/nyc timmam/pg_tileserv:Katacoda```{{execute}}

Now, if you look at the pg_tileserv tab again, you'll see the default UI and all of the nyc data being delivered as vector tiles. If you click on the preview link, you can see the vector tiles. You can also click on any of the vector tiles and see all of the attribute information contained in the database. 