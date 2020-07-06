All of the commands on this page should be run within Terminal 2.

To create a user-defined function, first we must log into the running database.

>**WAIT!** Before you execute the code block below, make sure you've navigated back to ```Terminal 2```. 

```PGPASSWORD="password" psql -h localhost -U groot nyc```{{execute}}

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
```{{execute}}

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
```{{execute}}

Now, go back to the pg_tileserv tab and you should now see two new functions.

