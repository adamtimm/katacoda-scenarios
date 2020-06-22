# Web Feature Service

Put simply, [Web Feature Services](https://www.ogc.org/standards/wfs) are a way to query and display data with spatial attributes through a web interface. Tradional web architectures have required a heavy middle tier software application to connect and configure the serving of your spatial data. These traditional architectures don't lend themselves to modern container platforms.  

# pg_featureserv

Pg_featureserv is a lightweight RESTful geospatial feature server for [PostGIS](https://postgis.net/), written in [Go](https://golang.org/).
It supports the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) REST API standard.

## Features

* Implements the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) standard.
  * Supports standard request parameters `limit`, `bbox`, property filtering
  * Extended parameters include `offset`, `properties`, `orderBy`, `transform`, `precision`
* Data reponses are formatted in JSON and GeoJSON
* Provides a simple HTML user interface, with web maps to view spatial data
* Uses the power of PostgreSQL to reduce the amount of code
  and to make data definition easy and familiar.
  * Feature collections are defined by database objects (tables and views)
* Uses PostGIS to provide geospatial functionality:
  * Transforming geometry data into the output coordinate system
  * Marshalling feature data into GeoJSON
* Full-featured HTTP support
  * CORS support with configurable Allowed Origins
  * GZIP response encoding

