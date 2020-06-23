To create a userdefined function, first we must log into the running database

```PGPASSWORD="password" psql -h localhost -U postgres nyc```{{execute}}



In order to have pg_featureserv publish a user defined function, we first need to create the schema that pg_featureserv looks for (note: we are planning to expand the schemas available to pg_featureserv, but for now it is limited to this schema). The schema needs to be called ```postgisftw``` (aka "postgis for the web")

```CREATE SCHEMA postgisftw;```{{execute}}

Now that we've created the schema, we can add our function. This function does a query aganist US Census Block borough names and returns the name of the subway station and Census Block geomtries that intersect with the subway geometry.

```
CREATE or REPLACE FUNCTION postgisftw.nyc_katacoda(boro_name character varying DEFAULT 'Bronx Park East')
RETURNS TABLE (station_name character varying, geom geometry)
AS $$
BEGIN
RETURN QUERY
SELECT a.boroname, a.geom 
FROM nyc_census_blocks a, nyc_subway_stations t
WHERE t.name= boro_name
AND ST_Intersects(ST_Buffer(a.geom,30),t.geom);
END;
$$
PARALLEL SAFE
STABLE
LANGUAGE plpgsql;
ANALYZE;
SELECT * FROM postgisftw.nyc_katacoda('Bronx Park East');``` 
{{execute}}

Now if you return to the pg_featureserv tab, you can look under the functions link and you'll see your newly created function.

To show it's live, let's get some borough names from our US Census Block table

```SELECT boroname FROM public.nyc_census_blocks ORDER BY gid ASC LIMIT 20;```{{execute}}
