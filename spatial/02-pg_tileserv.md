Pg_tileserv is a lightweight service that enables develoeprs to more easily access the vector tile generation capability in PostGIS via an open API. It is designed to run in both a "cloud native" environment such as Kubernetes, as well as a more traditional server architecture. 

This exercise will show you the steps to take to add pg_tileserv to your PostGIS implementation. 

To add pg_tileserv to your PostGIS database, you need to either download the [source code](https://github.com/CrunchyData/pg_tileserv), download the binaries, or one of our supported containers. We've already pre-staged the container of pg_tileserv for this scenario. 

To add the container to your postgis implentation, you'll need the connection info and username and password. 

```docker run -p 7800:7800 --env=DATABASE_URL=postgres://groot:password@172.18.0.2/nyc timmam/pg_tileserv:Katacoda```{{execute}}

"environment": {
    "showdashboard": true,
    "dashboards": [{"name": "URL", "href": "http://localhost:7800"},
        {"name": "Port 7800", "port": 7800},
        {"name": "Placeholder", "href": "https://[[HOST_SUBDOMAIN]]-7800-[[KATACODA_HOST]].environments.katacoda.com"}],
    "uilayout": "terminal-iframe"
}

