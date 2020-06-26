# pg_featureserv

Pg_featureserv is a lightweight RESTful geospatial feature server for [PostGIS](https://postgis.net/), written in [Go](https://golang.org/).
It supports the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) REST API standard.

Pg_featureserv implements the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) standard. It uses PostGIS to provide geospatial functionality:
  * Transforming geometry data into the output coordinate system
  * Marshalling feature data into GeoJSON

Pg_featureserv requires PostGIS 2.4 or greater to operate.

That's great, but I'm sure you're asking "What does it do? How does it work?" pg_featureserv works by 1) taking an http request and converting it to the neccessary SQL to have PostGIS return the spatial data as a feature. 2) it connects to the target PostGIS database via a Database URL connection string. It runs in a completely stateless manner, any configuration of the datalayers is driven by the underlying database. This means that data scientists, analysts and stewards can focus on maintaining their data and data structures. 

Now, let's get to showing you how easily it can be added to your PostGIS implementation and expose features. 

