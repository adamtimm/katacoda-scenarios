All of the commands on this page should be run within Terminal 2.

To create a user-defined function, first we must log into the running database.

>**WAIT!** Before you execute the code block below, make sure you've navigated back to ```Terminal 2```. 

```PGPASSWORD="password" psql -h localhost -U groot nyc```{{execute}}

Before we do that, let's take a quick look at the data. 

First, let's take a look at what tables we have.

```SELECT * FROM pg_catalog.pg_tables WHERE schemaname = 'public' AND schemaname != 'information_schema' ;```{{execute}} 

You can see we have six tables with information about New York City. If you look at the pg_tileserv tab > Table Layers, those same tables are also available in the pg_tileserv UI. 

### A bit of background on census data

The US Census Bureau is responsible for conducting the census of the US population every 10 years. (We just had one in 2020.) This information is then used by the US government to make all sorts of decisions to regarding the use and dispersment of tax dollars. But did you know they also make all this information publicly available? You can download this data and use it for your own analysis. 

The query in the funciton below could potentially be the first step in determining the population demographic (i.e. age, race, gender, etc) around certain subway stops within a walking radius. You could then add additional queries to further analyze the data (i.e. median household income in the area, time table of when the subway stops, peak rush hour traffic, etc)

Now, let's go back to the **Terminal 2** tab, and we'll take a quick look at one of the tables we'll use in the function.

```SELECT DISTINCT * FROM public.nyc_subway_stations ORDER BY name ASC LIMIT 10;```{{execute}}

You can see there is a variety of data in the table, but we will only use a subesection of this table and the US Census Block table. Now, let's get back to the function.

## Create a user-defined function

For this part of the exercise, we'll use a couple of the functions from Paul Ramsey's blog post on [Serving Tiles with Dynamic Geometry](https://info.crunchydata.com/blog/tile-serving-with-dynamic-geometry).

First we need to create the hexagons.

```
CREATE OR REPLACE 
FUNCTION 
hexagon(i integer, j integer, edge float8) 
RETURNS geometry 
AS $$ 
DECLARE 
h float8 := edge*cos(pi()/6.0); 
cx float8 := 1.5*i*edge; 
cy float8 := h*(2*j+abs(i%2)); 
BEGIN 
RETURN ST_MakePolygon(ST_MakeLine(ARRAY[ 
            ST_MakePoint(cx - 1.0*edge, cy + 0), 
            ST_MakePoint(cx - 0.5*edge, cy + -1*h), 
            ST_MakePoint(cx + 0.5*edge, cy + -1*h), 
            ST_MakePoint(cx + 1.0*edge, cy + 0), 
            ST_MakePoint(cx + 0.5*edge, cy + h), 
            ST_MakePoint(cx - 0.5*edge, cy + h), 
            ST_MakePoint(cx - 1.0*edge, cy + 0) 
         ])); 
END; 
$$ 
LANGUAGE 'plpgsql' 
IMMUTABLE 
STRICT 
PARALLEL SAFE;
```
{{execute}}

Then we'll fill tiles with those Hexagons.

```
CREATE OR REPLACE 
FUNCTION hexagoncoordinates(bounds geometry, edge float8, 
                            OUT i integer, OUT j integer) 
RETURNS SETOF record 
AS $$ 
    DECLARE 
        h float8 := edge*cos(pi()/6); 
        mini integer := floor(st_xmin(bounds) / (1.5*edge)); 
        minj integer := floor(st_ymin(bounds) / (2*h)); 
        maxi integer := ceil(st_xmax(bounds) / (1.5*edge)); 
        maxj integer := ceil(st_ymax(bounds) / (2*h)); 
    BEGIN 
    FOR i, j IN 
    SELECT a, b 
    FROM generate_series(mini, maxi) a, 
         generate_series(minj, maxj) b 
    LOOP 
       RETURN NEXT; 
    END LOOP; 
    END; 
$$
LANGUAGE 'plpgsql' 
IMMUTABLE 
STRICT 
PARALLEL SAFE;
```
{{execute}}

Now, go back to the pg_tileserv tab and you should now see two new functions.

