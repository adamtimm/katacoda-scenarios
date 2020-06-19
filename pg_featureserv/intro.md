# Spatial Relationships

This scenario gives you an introduction to pg_featureserv.

To use pg_featureserv, you need to have a PostGIS database with spatial data loaded in it. The database has already been started and the spatial data has already been loaded. This scenario will use data from New York City (NYC). This exercise shows how to connect pg_featureserv to your database and shows the resulting features. We also show an example of using user defined function through pg_featureserv. If you would like to learn more about creating funcitons, please review some of our other exercies. Data from this scenario is used in our other exercises as well, but the environements don't persist between scenarios.

We have already logged you into the PostgreSQL command line but, if you get disconnected here are the details on the database we are connecting to:

1. Username: groot
2. Password: password (same password for the postgres user as well)
3. A database named: nyc

And with that, let's dig in.
