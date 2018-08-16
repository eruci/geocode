# GeoCode
A Geolocation Code, mapping latitude,longitude to one alphanumeric number or three geonames.
==================

Geocode is a one-dimensional location code with several advantages over similar systems. It is shorter than most alternatives (up to 10 bytes), has higher accuracy (up to 1 meters) and avoids the borderline discontinuities of other one-dimensional location codes such a geohashes. 

It's human readable format, three-geo-names, uses three existing geographic place names in a hierarchical way with the first name representing a location name inside an approx 30 km^2 area containing the point, the second name covers about 0.2 km^2 and it also represents a location within the same area with a high probability. The third name is random and accounts for a very small area, approx 1 m^2.

For example,  34.03808,-118.30078 (a location in Los Angeles), is encoded to MZ8OSICO9M or as three geonames: LA-Hollywood-Moliterno. Another location nearby, say 34.03801,-118.30070, is MZ8OSICM94 or LA-Hollywood-NY. Unlike similar location encoding systems such as what3words, some of the words are intuitively connected to the place and some are not. This solves both the problems or error correction and human readable geographic proximity.

We use a simple space-filling technique to generate codes using an alphabet of all 36 alphanumeric characters or 179001 ascii geonames (selected from http://geonames.org and http://geonames.nga.mil/gns/html/gis_countryfiles.html with several requirements for names being recognizable, short and evenly spread throughout the earth.)

Geocode is designed for representing locations with high accuracy. It can be used as a high accuracy postal code system or for shortening latitude,longitude points with no loss of location information. (the three dimensional version also supports elevation, at a cost of one more byte in the resulting geocode).

Unlike grid-based location codes, geocodes represent points, not areas. Each geocode corresponds to a latitude,longitude pair with accuracy up to the 5th decimal point (i.e. 1 meter)

Latitude,longitude values are combined as two linear curves, then converted to binary numbers and their bits interwoven into one single number. As a result of this technique, similar geocodes are located geographically close together in both alphanumeric and 3geoname formats.

A geolocation expressed as (latitude,longitude) can be converted into a geocode, and vice versa using a data structure embeded in the software. Therefore this system can run offline.

The software is provided in the public domain to be used without any restrictions.


Links
-----
 * [Geocode.xyz](https://geocode.xyz/)
 * [Mailing list](https://groups.google.com/forum/#!forum/geocode)
 * [A lit review](https://github.com/eruci/geocode/wiki/Comparison-to-similar-systems)
 * [Geocode definition](https://github.com/eruci/geocode/wiki/Geocode)

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
   
This solves a borderline case of the popular geohash algorithm. Geohashes of (45.00001,-64.36000) and (44.99999,-64.36000) are f840p2n2p3 and dxfpzryrzq respectively although the points are only 1 meter apart. (see http://geohash.org/f840p2n2p3  and http://geohash.org/dxfpzryrzq )
   
A more detailed description of the algorithm and a comparison to similar system is provided in the wiki.

Example Code
------------
Coming soon.


Todo
#geo3names
Geo3Names - The Geolocation Code mapping latitude,longitude to 3 intuitive geonames.
