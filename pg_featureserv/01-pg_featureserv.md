# pg_featureserv

Pg_featureserv is a lightweight RESTful geospatial feature server for [PostGIS](https://postgis.net/), written in [Go](https://golang.org/).
It supports the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) REST API standard.

Pg_featureserv implements the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) standard. It uses PostGIS to provide geospatial functionality:
  * Transforming geometry data into the output coordinate system
  * Marshalling feature data into GeoJSON

Pg_featureserv requires PostGIS 2.4 or greater to operate.

