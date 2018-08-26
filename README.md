# GeoCode
A Geolocation Code, mapping (latitude,longitude) to {(one alphanumeric number) or (three geonames)}.
==================



## Abstract

Encoding geographic coordinates into a string is a trivial thing. 
Yet, there are many grid based systems (geohash, PlusCodes, Mapcodes), and some even turn the thing into a business (Zippr, What3Words). 
I agree with the commonly stated motivation that Latitude and Longitude are not sufficient for identifying a place in both an unambiguous and human friendly way. A single string for this pair of numbers is a better representation, if only it can preserve all the information contained in the original latitude,longitude pair, something no existing geo-encoding system does. That's my goal.


## Intro

Geocode is a one-dimensional location code. It uses a simple space-filling technique to map a two dimensional point (latitude,longitude) to either an alphanumeric string or a geoname triple with no loss of information.

Geocode has several advantages over similar systems. 

The alphanumeric geocode is short (10 bytes), has higher accuracy (up to 1 meters) and avoids the borderline discontinuities of other one-dimensional location codes such as geohashes. 

Triple geoname codes on the other hand are more memorizable, are intuitively reprentative of the location and are composed of relatively short existing geo names (up to 8 letters).

The first name in a triple geoname code represents the most prominent location name inside a 21,403 km^2 area containing the point.

For example,  34.05223,-118.24368 (a location in Los Angeles), is encoded to EKEAJ18E08 or as three geonames: LA-GASPAR-YANSI. Another location about 1 m away, say (34.05223,-118.24369), is EKEAJ18E1D or LA-GASPAR-HINGWEN.

The human readable algorithm uses 146300 geonames from http://geonames.org and http://geonames.nga.mil/gns/html/gis_countryfiles.html with several requirements for the names (chosen to be recognizable, short, easy to pronounce, distinct from each other and evenly spread throughout the earth.) We also shorten geonames to their acronyms whenever possible (LA -> Los Angeles, NY -> New York, etc)

Unlike many grid-based location codes, geocodes represent points not areas. Each geocode maps to a latitude,longitude pair with accuracy up to the 5th decimal point (i.e. 1 meter)

In a nutshell, latitude,longitude values are represented as two linear curves, converted to binary numbers then combined by interleaving their bits. As a result of this technique, similar geocodes are located geographically close together in both alphanumeric and triple geoname formats.

A geolocation expressed as (latitude,longitude) can be converted into a geocode, and vice versa using a data structure embeded in the software.

The software is in the public domain to be used without any restrictions.


Links
-----
 * [Geocode.xyz](https://geocode.xyz/)
 * [3geonames.org](https://3geonames.org/)
 * [Mailing list](https://groups.google.com/forum/#!forum/geocode)
 * [A lit review](https://github.com/eruci/geocode/wiki/Comparison-to-similar-systems)


In depth description
-----------
A Geocode is a case insensitive string that comes in two forms: as a 10 byte long alpanumeric string or as 3 geonames separated by dashes. Let's call them Alphanumeric Geocodes or Triple Name Geocodes.

An alphanumeric geocode has length of 10 bytes for any location on earth, with locations that are far having very distinct geocodes, while those that are near sharing most significant bits. 

The minimum value for a GeoCode is in the South Pole (-90.00000,-154.23359)/(1000000000)/(SOUTH-NOALA-MONETTE), and the maximum is at the North Pole (90.00000,23.14496)/(UTRC9O4N8P)/(NORTH-BUOF-SCHONGAU). These points are the computed minimum and maximum values for the space-fitting function I'm using to map latitude,longitude to a single value for both.

Every other location on earth will fall between these values.

For example, the point 0.00000,0.00000 at the intersection the equator and greenwich is alphanumeric geocode 7NFJIIDSBT or triple name geocode EQU-NDOLA-ALAKH, whereas -43.95296,-176.54867 at [178 Waitangi Wharf Owenga Road, Chatham Islands, New Zealand](https://geocode.xyz/178%20Waitangi%20Wharf%20Owenga%20Road,%20Chatham%20Islands,%20Ch%20%20New%20Zealand) is alphanumeric geocode 8VEB9501G0 and triple name geocode WAITANGI-USAKOS-IWHR)

An alphanumeric geocode in most cases is half as long as its corresponding latitude,longitude and preserves all positional properties of the latitude,longitude pair. 

||||||||||
-43.95296,-176.54867
8VEB9501G0||||||||||

Similarly, a Triple Name Geocode is composed of three existing geonames of length no more than 8 bytes, with the first geoname being the most promiment location name in its geographic proximity.

Alphanumeric Geocodes and Triple Name Geocode can not be shortened nor truncated because they are basically either a base 36 alphanumeric representation of a single number representing both latitude and longitude, or a base 146300 name alphabet encoding. 

Alphanumeric Geocodes at borderline areas will share most of the significant digits.
   * (45.00001,-64.36000) -> EHB105754C -> HALIFAX-GAZAH-DOMOU
   * (44.99999,-64.36000) -> EHB1056SH4 -> HALIFAX-GAZAH-NDITI
   
This solves the many borderline issues of the popular geohash algorithm. 

Equivalent Geohashes are:
   * (45.00001,-64.36000) -> f840p2n2p3 
   * (44.99999,-64.36000) -> dxfpzryrzq 
   
Although these points are only 1 meter apart. (see http://geohash.org/f840p2n2p3  and http://geohash.org/dxfpzryrzq )
   
A more detailed comparison to similar system is provided in the wiki.

Example Code
------------
Coming soon.


Todo
#geo3names
Geo3Names - The Geolocation Code mapping latitude,longitude to 3 intuitive geonames.
