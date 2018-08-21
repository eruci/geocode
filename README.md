# GeoCode
A Geolocation Code, mapping (latitude,longitude) to {(one alphanumeric number) or (three geonames)}.
==================



## Abstract

Encoding geographic coordinates into a string is a trivial thing. Yet, there are many grid based systems (geohash, PlusCodes, Mapcodes and many many more), and some even turn the thing into a business (Zippr, What3Words and clones). We agree with the commonly stated motivation that Latitude and Longitude are not sufficient for identifying a place in both an unambiguous and human friendly way. A single string for this pair of numbers is a more desirable representation, if only it can preserve all the information contained in the original latitude,longitude pair, something no geo-encoding system currently does. This is our goal.


## Intro

Geocode is a one-dimensional location code. It uses a simple space-filling technique to map a two dimensional point (latitude,longitude) to either an alphanumeric string or a geoname triple with no loss of information.

Geocode has several advantages over similar systems. The alphanumeric geocode is short (up to 10 bytes), has higher accuracy (up to 1 meters) and avoids the borderline discontinuities of other one-dimensional location codes such as geohashes. Triple geoname codes on the other hand are more memorizable, are intuitively reprentative of the location and are composed of relatively short geo names (up to 8 letters).

The geocode of a point in its human readable format uses three existing geographic place names in a hierarchical way, with the first name representing the most prominent location name inside a 22,500 km^2 area containing the point, while the other two names are not necessarily intuitively connected to the place.

For example,  34.03808,-118.30078 (a location in Los Angeles), is encoded to MZ8OSICO9M or as three geonames: LA-Rome-Moliterno. Another location nearby, say 34.03801,-118.30070, is MZ8OSICM94 or LA-Hollywood-NY.

The human readable algorithm uses 163000 geonames from http://geonames.org and http://geonames.nga.mil/gns/html/gis_countryfiles.html with several requirements for the names (mainly they must be recognizable, short, easy to pronounce, distinct from each other and evenly spread throughout the earth.)

Unlike many grid-based location codes, geocodes represent points not areas. Each geocode maps to a latitude,longitude pair with accuracy up to the 5th decimal point (i.e. 1 meter)

Latitude,longitude values are combined as two linear curves, converted to binary numbers then combined by interleaving their bits into one single binary number. As a result of this technique, similar geocodes are located geographically close together in both alphanumeric and triple geoname formats.

A geolocation expressed as (latitude,longitude) can be converted into a geocode, and vice versa using a data structure embeded in the software. Therefore this system can run offline.

The software is in the public domain to be used without any restrictions.


Links
-----
 * [Geocode.xyz](https://geocode.xyz/)
 * [Mailing list](https://groups.google.com/forum/#!forum/geocode)
 * [A lit review](https://github.com/eruci/geocode/wiki/Comparison-to-similar-systems)
 * [Geocode definition](https://github.com/eruci/geocode/wiki/Geocode)

In depth description
-----------
Geocodes come in two forms: as alpanumeric strings up to 10 bytes, or as human readable 3 geonames separated by dashes. We will be referring to them as either Alphanumeric Geocodes or Triple Name Geocodes.

An alphanumeric geocode ranges in length from 1 (the point 0.00000,0.00000 at the intersection the equator and greenwich is geocode 0), to length 10 (-43.95296,-176.54867 at [178 Waitangi Wharf Owenga Road, Chatham Islands, New Zealand](https://geocode.xyz/178%20Waitangi%20Wharf%20Owenga%20Road,%20Chatham%20Islands,%20Ch%20%20New%20Zealand) is geocode QH0VYUJ3M7 )

As Triple Name Geocode 0.00000,0.00000 is ZERO-ZERO-AFRICA , and -43.95296,-176.54867 CHATHAM-PUNE-NIL.

An alphanumeric geocode in most cases is half as long as its corresponding latitude,longitude and preserves all properties of the latitude,longitude pair. 

Similarly, a Triple Name Geocode is composed of three existing geonames of length no more than 8 bytes, with the first geoname being the most promiment location name in its geographic proximity.

Alphanumeric Geocodes and Triple Name Geocode can not be shortened nor truncated because they are basically either a base 36 alphanumeric representation of a single number representing both latitude and longitude, or a base 163000 word alphabet encoding of this number. 

Alphanumeric Geocodes at borderline areas will share most of the significant digits.
   * (45.00001,-64.36000) -> 2QGD21BLIJ
   * (44.99999,-64.36000) -> 2QGD21C09J
  
Three Name Geocodes at borderline areas will share the first name.
   * (45.00001,-64.36000) -> HALIFAX-NY-RIO
   * (44.99999,-64.36000) -> HALIFAX-BAKSAN-MERZEN
   
This solves the many borderline issues of the popular geohash algorithm. Geohashes of (45.00001,-64.36000) and (44.99999,-64.36000) are f840p2n2p3 and dxfpzryrzq respectively although the points are only 1 meter apart. (see http://geohash.org/f840p2n2p3  and http://geohash.org/dxfpzryrzq )
   
A more detailed description of the algorithm and a comparison to similar system is provided in the wiki.

Example Code
------------
Coming soon.


Todo
#geo3names
Geo3Names - The Geolocation Code mapping latitude,longitude to 3 intuitive geonames.
