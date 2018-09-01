# GeoCode
An open source Geolocation Code, mapping (latitude,longitude) to {(one alphanumeric number) or (three geonames)}.
==================

[Demo](https://3geonames.org/): https://3geonames.org/

## Abstract

Encoding geographic coordinates into a string is a trivial thing. 
Yet, there are many grid based systems (geohash, PlusCodes, Mapcodes), and some even turn the thing into a business (Zippr, What3Words). 
I agree with the commonly stated motivation that Latitude and Longitude are not sufficient for identifying a place in both an unambiguous and human friendly way. A single string for this pair of numbers is a better representation, if only it can preserve all the information contained in the original latitude,longitude pair, something no existing geo-encoding system does. That's my goal.


## Intro

Geocode is a one-dimensional location code. It uses a simple space-filling technique to map a two dimensional point (latitude,longitude) to either an alphanumeric string or a geoname triple with no loss of information. It is a one-to-one mapping (no two geocodes map to the same point and no two points map to the same geocode).

Geocode has several advantages over similar systems. 

The alphanumeric geocode is short (10 bytes), has higher accuracy (up to 1 meters) and avoids the borderline discontinuities and many to one mappings of other one-dimensional location codes such as geohashes. 

Triple geoname codes on the other hand are more memorizable, are intuitively reprentative of the location and are composed of relatively short existing geo names (up to 8 letters).

The first name in a triple geoname code represents the most prominent location name inside a 21,403 km² area containing the point.

For example,  [34.05223,-118.24368](https://3geonames.org/34.05223,-118.24368) (a location in Los Angeles), is encoded to [EKEAJ18E08](https://3geonames.org/EKEAJ18E08) or as three geonames: [LA-GASPAR-YANSI](https://3geonames.org/LA-GASPAR-YANSI) or as a hybrid code [LA-MJKQH4](https://3geonames.org/LA-MJKQH4). Another location about 1 m away, say (34.05223,-118.24369), is EKEAJ18E1D or LA-GASPAR-HINGWEN or LA-MJKQI9.

The human readable algorithm uses 146300 geonames from http://geonames.org and http://geonames.nga.mil/gns/html/gis_countryfiles.html with several requirements for the names (chosen to be recognizable, short, easy to pronounce, distinct from each other and evenly spread throughout the earth.) We also shorten geonames to their acronyms whenever possible (LA -> Los Angeles, NY -> New York, etc)

Unlike many grid-based location codes, geocodes represent points not areas. Each geocode maps to a latitude,longitude pair with accuracy up to the 5th decimal point (i.e. 1 meter)

In a nutshell, latitude,longitude values are represented as two linear curves, converted to binary numbers then combined by interleaving their bits. As a result of this technique, similar geocodes are located geographically close together in both alphanumeric and triple geoname formats.

A geolocation expressed as (latitude,longitude) can be converted offline into a geocode, and vice versa using a data structure embeded in the software.

The software is in the public domain to be used without any restrictions.


Links
-----
 * [Geocode.xyz](https://geocode.xyz/)
 * [3geonames.org](https://3geonames.org/)
 * [Mailing list](https://groups.google.com/forum/#!forum/geocode)
 * [A lit review](https://github.com/eruci/geocode/wiki/Comparison-to-similar-systems)


In depth description
-----------
A Geocode is a case insensitive string that comes in three forms: as a 10 byte long alpanumeric string or as 3 geonames separated by dashes or as a hybrid code composed of one geoname and a string. Let's call them Alphanumeric Geocodes or Triple Name Geocodes or Hybrid Geocodes.

An alphanumeric geocode has length of 10 bytes for any location on Earth, with locations that are far having very distinct geocodes, while those that are near sharing most significant digits/names. 

The minimum value for a GeoCode is in the South Pole:

   [-90.00000,-154.23359](https://3geonames.org/-90.00000,-154.23359) -> 1000000000 -> SOUTH-NOALA-MONETTE -> SOUTH-9KUBWJK
    
The maximum is at the North Pole:

   [90.00000,23.14496](https://3geonames.org/90.00000,23.14496) -> UTRC9O4N8P -> NORTH-BUOF-SCHONGAU -> NORTH-6K9GURD
    
These points are the computed minimum and maximum values for the Space-filling function I'm using.

Every other location on earth will fall in between these values.

For example, the point at the intersection the Equator and Greenwich is:

   [0.00000,0.00000](https://3geonames.org/0.00000,0.00000) -> 7NFJIIDSBT -> EQU-NDOLA-ALAKH -> EQU-2AT2YT5
    
While another location further away, say [178 Waitangi Wharf Owenga Road, Chatham Islands, New Zealand](https://geocode.xyz/178%20Waitangi%20Wharf%20Owenga%20Road,%20Chatham%20Islands,%20Ch%20%20New%20Zealand) is:

   [-43.95296,-176.54867](https://3geonames.org/-43.95296,-176.54867) -> 8VEB9501G0 -> WAITANGI-USAKOS-IWHR -> WAITANGI-1FI7H9S
    
A triple name geocode or a hybrid geocode can be shortened to represent an area.
![WAITANGI](https://raw.githubusercontent.com/eruci/geocode/master/waitangi.png)

The polygons are not squares, due to the a variation of the Z-order curve used to represent the points.

![WAITANGI-USAKOS](https://raw.githubusercontent.com/eruci/geocode/master/waitangi-usakos.png)

The smaller polygon (WAITANGI-USAKOS in this case) area is approx. 0.1463 km².

An alphanumeric geocode in most cases is half as long as its corresponding latitude,longitude and preserves all positional properties of the latitude,longitude pair in its unshortened 10-byte form. 

    * ||||||||||
    * -43.95296,-176.54867
    * 8VEB9501G0||||||||||

Alphanumeric Geocodes and Triple Name Geocode represent points in their unshortened form, but if they are shortened they represent areas ranging from 21,403 km² (the first name in a triple name geocode) to 0.1463 km² (the first and second name in a triple name geocode). Alphanumeric Geocodes can be shortened too, but it does not make a lot of sense to do so. The first letter in an Alphanumeric Geocode accounts for 14.17 million km².

Alphanumeric Geocodes/Triple Name Geocodes at borderline areas will share most of the significant bytes/geonames.
   * (45.00001,-64.36000) -> EHB105754C -> HALIFAX-GAZAH-DOMOU
   * (44.99999,-64.36000) -> EHB1056SH4 -> HALIFAX-GAZAH-NDITI
   
This solves the many borderline issues of the popular geohash algorithm. 

Equivalent Geohashes are:
   * (45.00001,-64.36000) -> f840p2n2p3 
   * (44.99999,-64.36000) -> dxfpzryrzq 
   
Although these points are only 1 meter apart. (see http://geohash.org/f840p2n2p3  and http://geohash.org/dxfpzryrzq )

Triple Name Geocode also solves what is widely perceived as a flaw with similar systems such as What3words, in that the first name is a geoname in close proximity to the point. This gives intuitive context to the association. 

In some cases a Hybrid Geocode is more readable, with the first geoname representing the general area the point is in.

Here are all the Geocodes around the point above. 

| Triple Geoname  | Alphanumeric | Hybrid |
| ------------- | ------------- | ------------- |
| HALIFAX-GAZAH-MOUFA  | EHB1057549  | HALIFAX-6UMXPAL  |
| HALIFAX-GAZAH-DOLAM  | EHB105754D  | HALIFAX-6UMXPAL  |
| HALIFAX-GAZAH-URSS	| EHB105754L	| HALIFAX-6UMXPAT    |
| HALIFAX-GAZAH-NAITA	| EHB1056WOW	| HALIFAX-6UMXGV4 |
| HALIFAX-GAZAH-KUMON	| EHB105754G	| HALIFAX-6UMXPAO |
| HALIFAX-GAZAH-IGNYANDA	| EHB105754H	| HALIFAX-6UMXPAP |
| HALIFAX-GAZAH-GOGMI	| EHB105754K	| HALIFAX-6UMXPAS |
| HALIFAX-GAZAH-SENHA	| EHB10570WK	| HALIFAX-6UMXL2S |

A more detailed comparison to similar systems with more examples is provided in the [wiki](https://github.com/eruci/geocode/wiki/Comparison-to-similar-systems).

