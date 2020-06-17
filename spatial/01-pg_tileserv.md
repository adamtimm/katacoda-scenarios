Vector tiles are a bandwidth efficient method of transferring spatial data over the web for display in webmaps. For a brief history of webmaps and vector tiles, watch [Paul Ramsey's presentation at PostGIS Day 2019](https://youtu.be/t8eVmNwqh7M "PostGIS Day 2019 Vector Tiles"). 

The original method for generation and serving of vector tiles involved some type of batch data processing of the vector tiles and then hosting those in a web cache. Depending on the amount of data, this process could be time consuming. Additionally, if your dataset is constantly changing, or only changes slightly, you are doing a lot of processing on data that hasn't changed, just to update a small part of the dataset. 

With the release of PostGIS 2.4, PostGIS introduced the ability to generate vector tiles directly from the database. This provided developers with the ability to have constantly up-to-date vector tiles in their webmap. However, it required the developers to write their own API to leverage this functionality. 

pg_tileserv is a lightweight service that enables develoeprs to more easily access the vector tile generation capability in PostGIS via an open API. It is designed to run in both a "cloud native" environment such as Kubernetes, as well as a more traditional server architecture. 

This exercise will show you the steps to take to add pg_tileserv to your PostGIS implementation. 