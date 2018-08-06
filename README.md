# Geocode
A Geolocation Code mapping latitude,longitude to one alphanumeric number.
==================

Geocode is a one-dimensional location code with several advantages over similar systems. It is shorter than most alternatives (up to 10 bytes), has higher accuracy (up to 1 meters) and avoids the borderline discontinuities of other one-dimensional location codes such a geohashes and plus codes. 

This system uses a simple space-filling technique to generate codes using an alphabet of all 36 alphanumeric characters.

It is designed to generate single geocodes for representing locations with high accuracy. It can be used as a high accuracy postal code system or for shortening latitude,longitude points with minimal loss of location information. (the three dimensional version also supports elevation, at a cost of one more byte in the resulting geocode).

Departing from the overriding techique of many grid-based location codes, geocodes represent points, not areas. Each geocode corresponds to a latitude,longitude pair with accuracy up to the 5th decimal point (i.e. 1 meter)

Latitude,longitude values are combined as two linear curves, then converted to binary numbers and their bits interwoven into one single number. As a result of this technique, similar geocodes are located geographically close together.

A geolocation expressed as (latitude,longitude) can be converted into a geocode, and vice versa without the help of a database. Therefore this system can run offline.

A very simple algorithm converting between coordinates to base 36 alphanumeric geocodes and vice versa, is provided in the public domain to be used without any restrictions.


Links
-----
 * [Geocode.xyz](https://geocode.xyz/)
 * [Mailing list](https://groups.google.com/forum/#!forum/geocode)
 * [An overview of similar systems](https://groups.google.com/forum/#!forum/geocode)
 * [Geocode definition](https://github.com/geocode/geocode_definition.adoc)

Description
-----------
Geocodes are alphanumeric strings built from an alhpabet of 26 ASCII letters and 10 digits. All digits in the code alternate between latitude and longitude. 

A geocode ranges in length from 1 (the point 0.00000,0.00000 at the intersection the equator and greenwich is geocode 0), to length 10 (-43.95296,-176.54867 at [178 Waitangi Wharf Owenga Road, Chatham Islands, New Zealand](https://geocode.xyz/178%20Waitangi%20Wharf%20Owenga%20Road,%20Chatham%20Islands,%20Ch%20%20New%20Zealand) is geocode QH0VYUJ3M7 )

Latitude,longitude up to the 5th decimal point can be writen with a minimum of 3 digits (0,0) and up to 20 digits (-43.95296,-176.54867) as a pair of decimal numbers.

A geocode in most cases is half as long as its corresponding latitude,longitude and preserves many properties of the latitude,longitude pair to a very large extent. 

Geocodes can not be shortened or truncated because they are basically a base 36 alphanumeric representation of a single number representing both latitude and longitude (with their digits interwoven )

Geocodes at borderline areas will share most of the significant digits.
   * (45.00001,-64.36000) -> 2QGD21BLIJ
   * (44.99999,-64.36000) -> 2QGD21C09J
   
This solves a borderline case of the popular geohash algorithm. Geohashes of (45.00001,-64.36000) and (44.99999,-64.36000) are f840p2n2p3 and dxfpzryrzq respectively although the points are only 1 meter appart. (see http://geohash.org/f840p2n2p3  and http://geohash.org/dxfpzryrzq )
   
A more detailed description of the algorithm and a comparison to similar system is provided in the wiki.

Example Code
------------
Coming soon.


Todo
#geo3names
Geo3Names - The Geolocation Code mapping latitude,longitude to 3 intuitive geonames.
