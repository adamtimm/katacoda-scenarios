Now we want to give you a short tour of the pg_featureserv UI. First and foremost, the intent of the UI is to allow the user to quickly confirm the data is available through the service. It is not meant to be a finished UI for an end user application. With that in mind, click on the ```pg_featureserv``` tab and we'll take a look around.

# OpenAPI Schema and Conformance

If you click on ```OpenAPI Schema``` it will take you to the built in Swagger documentation for the full API spec.

The conformance link will take you to the list of links that describe the OGC API for Features spec.

# Collections and Functions

Collections will list all of the tables that contain a valid geometry column(```geom```) that the pg_featureserv account has permissions to. If a tabel is missing from this view that you were expecting to see do the following troubleshooting steps.

1) Verify user roles/permissions for the service account.
2) Verify the table has a geometry column and the geometry is valid. 


# json and view

