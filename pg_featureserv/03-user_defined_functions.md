# (Be sure to run these commands in Terminal 2)

To create a user defined function, first we must log into the running database

```PGPASSWORD="password" psql -h localhost -U groot nyc```{{execute}}



In order to have pg_featureserv publish a user defined function, we first need to create the schema that pg_featureserv looks for (note: we are planning to expand the schemas available to pg_featureserv, but for now it is limited to this schema). The schema needs to be called ```postgisftw``` (aka "postgis for the web")

```CREATE SCHEMA postgisftw;```{{execute}}

Now that we've created the schema, we can add our function. But before we do that, let's take a quick look at the data. 

First, let's take a look at what tables we have.

```SELECT * FROM pg_catalog.pg_tables WHERE schemaname = 'public' AND schemaname != 'information_schema' ;```{{execute}} 

You can see we have 6 tables with information about New York city. If you look at the pg_featureserv tab > collections, these tables are also available through pg_feautreserv. 

Now, let's take a quick look at one of the tables we'll use in the function.

```SELECT DISTINCT * FROM public.nyc_subway_stations ORDER BY name ASC LIMIT 10;```{{execute}}

You can see there is a variety of data in the table, but we will only use a subesection of this table and the US Census Block table. Now, let's get back to the function. 

This function does a query aganist US Census Block borough names and returns the name of the borough and Census Block geomtries that intersect with the subway geometry.

```CREATE or REPLACE FUNCTION postgisftw.nyc_katacoda(stop_name character varying DEFAULT 'Bronx Park East')
RETURNS TABLE (borough_name character varying, geom geometry)
AS $$
BEGIN
RETURN QUERY
SELECT a.boroname, a.geom 
FROM nyc_census_blocks a, nyc_subway_stations t
WHERE t.name= stop_name
AND ST_Intersects(ST_Buffer(a.geom,30),t.geom);
END;
$$
PARALLEL SAFE
STABLE
LANGUAGE plpgsql;```{{execute}}

Now if you return to the pg_featureserv tab, you can look under the functions link and you'll see your newly created function.

To show it's live, let's get some station names from our subway stations table

```SELECT DISTINCT name FROM public.nyc_subway_stations
ORDER BY name ASC LIMIT 40;```{{execute}}

You can grab one of these names and put it in the ```stop_name``` field on the pg_featureserv tab (click on ```view``` to view the output geometries).

