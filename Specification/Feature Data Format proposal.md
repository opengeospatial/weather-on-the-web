<div class="mume markdown-preview   ">

# Weather on the web Feature data format proposal

## 1 Abstract

The aim of Weather on the Web APIs SHOULD be to make Meteorological data accessible to as wide an audience as possible, currently there is no standard approach for describing weather information for a point or feature making it difficult for developers to utilise the data available.

Without clear and concise Metadata it is possible to misinterpret the information provided, it is essential that where possible the returned data should contain enough metadata so that it is self describing. This is further complicated by the nature of forecast data in that it is 'versioned', updated versions of the same information are provided when new models run.

Weather on the Web APIs are intended for developers who may not be familiar with the Meteorological domain and the APIs should not assume any Meteorological domain knowledge by the consumers, for the majority of the users of the APIs, weather will just be one of the inputs to their product.

This document establishes the guidelines the APIs SHOULD follow so interfaces are developed consistently.

## 2 Introduction

In order to support the widest range of languages and platforms Weather on the Web APIs will be provided via HTTP interfaces.

The benefits of consistency accrue in aggregate as well; consistency will allow developers to leverage common code, patterns, documentation and design decisions.

These guidelines aim to achieve the following:

*   Define consistent practices and patterns for all Weather on the web Feature API endpoints.
*   Make accessing Weather on the web interfaces easy for all application developers.
*   Allow developers to leverage the prior work on other services to implement, test and document endpoints defined consistently.

Any data format representing weather information must include information its time and location, currently one of the most popular (flexible) formats for describing location data is [GeoJSON](https://tools.ietf.org/html/rfc7946 "GeoJSON"), this document describes a proposed approach to extending [GeoJSON](https://tools.ietf.org/html/rfc7946 "GeoJSON") to support the Feature data returned by Weather on the web APIs.

The temporal information that any format returning Meteorological information MUST describe is when is the information returned valid from, when is it valid to and when to the information returned last changed.

## 3 Requirements language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",  
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and  
"OPTIONAL" in this document are to be interpreted as described in  
[RFC2119](https://tools.ietf.org/html/rfc2119).

The key word "LOCAL" refers to the location that the data represents (i.e. the location defined by the longitude and latitude of the GeoJSON feature object).

## 4 Data format

Weather on the web feature data format is based on GeoJSON but extends it to provide the extra metadata required to describe the information returned by the users query. The core parts of [GeoJSON](https://tools.ietf.org/html/rfc7946 "GeoJSON") object are as follows:

*   type

    <pre class="language-text"> The type of GeoJSON object, the default for Weather on the web feature data will be FeatureCollection
    </pre>

*   features

    <pre class="language-text"> An array of Feature objects which contain the data values. 
    </pre>

*   bbox

    <pre class="language-text"> A bounding box which defines an area which encompasses all of the features
     with the FeatureCollection.
    </pre>

Weather on the Web extends the format by adding two new values:

*   metadata

    <pre class="language-text">This defines when the data was created, links to Licencing details, 
    links to the terms and conditions and any text that should be included
    for attributions.
    </pre>

*   parameters

    <pre class="language-text"> An array of parameter objects, these follow the same specification as that defined for 
     CoverageJSON https://github.com/covjson/specification/blob/master/spec.md  
    </pre>

### 4.1 Definitions

*   JavaScript Object Notation (JSON), and the terms object, name, value, array, string, number, and null, are defined in [IETF RFC 4627](http://www.ietf.org/rfc/rfc4627.txt).

*   JSON-LD is defined in [http://www.w3.org/TR/json-ld/](http://www.w3.org/TR/json-ld/).

*   A compact URI has the form prefix:suffix.

*   Product is used to describe the GeoJSON object and the data contained within it.

### 4.2 i18n Objects

An i18n object represents a string in multiple languages where each key is a language tag as defined in [BCP 47](http://tools.ietf.org/html/bcp47), and the value is the string in that language.  
The special language tag `"und"` can be used to identify a value whose language is unknown or undetermined.

### 4.3 Type Object

String which describes the type of GeoJSON, for Weather on the web feature data this SHOULD be 'FeatureCollection'.

### 4.4 Features Array

An array of Feature objects which contain the data values.

### 4.5 Feature Object

From the GeoJSON specification:

A Feature object represents a spatially bounded thing. Every Feature  
object is a GeoJSON object no matter where it occurs in a GeoJSON  
text.

*   A `Feature` object has a `"type"` member with the value "Feature".

*   A `Feature` object has a member with the name `"geometry"`. The value  
    of the geometry member SHALL be either a Geometry object as  
    defined in [RFC 7946](https://tools.ietf.org/html/rfc7946#section-3.1), in the case that the Feature is unallocated, a JSON null value.

*   A Feature object has a member with the name `"properties"`. The value of the properties member is the JSON object which contains the data values for the product.

*   If a Feature has a commonly used identifier, that identifier  
    SHOULD be included as a member of the Feature object with the name  
    `"id"`, and the value of this member is either a JSON string or  
    number.

### 4.6 Properties Object

A Properties object contains all of the data values

*   A `Properties` object MUST have a `"Days"` member which is an JSONArray of Day objects.
*   All Dates defined within a Properties object SHOULD follow the same format convention either [RFC3339](https://tools.ietf.org/html/rfc3339) strings and the default representation SHOULD be UTC time plus LOCAL offset.

### 4.7 Day Object

A Day object contains all of the data values appropriate to the defined day.

*   A `Day` object MUST have a `"WxDate"` member which is Date defined as a [RFC3339](https://tools.ietf.org/html/rfc3339) string. The default representation SHOULD be UTC time plus LOCAL offset.

*   A `Day` object MUST represent the hours 00 to 23 LOCAL time for the latitude and longitude the data is representative of.

*   A `Day` object MUST have a `"Times"` member which is an JSONArray of `Time` objects.

*   A `Day` object MAY have a `"Sunrise"` member which is Date defined as a [RFC3339](https://tools.ietf.org/html/rfc3339) string and the default representation SHOULD be UTC time plus LOCAL offset. This is the time of Sunrise at the location defined for the feature.

*   A `Day` object MAY have a `"Sunset"` member which is Date defined either as a [RFC3339](https://tools.ietf.org/html/rfc3339) string and the default representation SHOULD be UTC time plus LOCAL offset. This is the time of Sunset at the location defined for the feature.

*   A `Day` object MAY have a `"Moonphase"` member which is a String value as defined in ([https://en.wikipedia.org/wiki/Lunar_phase](https://en.wikipedia.org/wiki/Lunar_phase)).

*   A `Day` object MAY have other members which have values which represent a value for the whole day period, these values must have entries in the Parameter section which provide the metadata to describe them.

### 4.8 Time Object

*   All Members of a `Time` object SHOULD be described in the Parameter object.

*   A `Time` object MUST have a `"valid_time"` member which is Date defined as a [RFC3339](https://tools.ietf.org/html/rfc3339) string. The default representation SHOULD be UTC time plus LOCAL offset.

*   A `Time` object MUST contain at least one of the core data values defined in Appendix A.

### 4.9 bbox Object

The `"bbox"` values define shapes with edges that follow lines of  
constant longitude, latitude, and elevation.

*   The bbox object MUST define an area which contains all of the coordinates defined within the FeatureCollection.
*   The bbox object MAY define the volume (i.e. include elevation values) which contains all of the coordinates defined within the Feature Collection.

### 4.10 Parameter Objects

Parameter objects represent metadata about the values of the feature in terms of the observed property (like water temperature), the units, and others.

*   A parameter object MAY have any number of members (name/value pairs).
*   A parameter object MUST have a member with the name `"type"` and the value `"Parameter"`.
*   A parameter object MAY have a member with the name `"id"` where the value MUST be a string and SHOULD be a common identifier.
*   A parameter object MAY have a member with the name `"label"` where the value MUST be an i18n object that is the name of the parameter and which SHOULD be short. Note that this SHOULD be left out if it would be identical to the `"label"` of the `"observedProperty"` member.
*   A parameter object MAY have a member with the name `"description"` where the value MUST be an i18n object which is a, perhaps lengthy, textual description of the parameter.
*   A parameter object MUST have a member with the name `"observedProperty"` where the value is an object which MUST have the member `"label"` and which MAY have the members `"id"`, `"description"` and `"categories"`. The value of `"label"` MUST be an i18n object that is the name of the observed property and which SHOULD be short. If given, the value of `"id"` MUST be a string and SHOULD be a common identifier. If given, the value of `"description"` MUST be an i18n object with a textual description of the observed property. If given, the value of `"categories"` MUST be a non-empty array of category objects. A category object MUST an `"id"` and a `"label"` member, and MAY have a `"description"` member. The value of `"id"` MUST be a string and SHOULD be a common identifier. The value of `"label"` MUST be an i18n object of the name of the category and SHOULD be short. If given, the value of `"description"` MUST be an i18n object with a textual description of the category.
*   A parameter object MAY have a member with the name `"categoryEncoding"` where the value is an object where each key is equal to an `"id"` value of the `"categories"` array within the `"observedProperty"` member of the parameter object. There MUST be no duplicate keys. The value is either an integer or an array of integers where each integer MUST be unique within the object.
*   A parameter object MAY have a member with the name `"unit"` where the value is an object which MUST have either or both the members `"label"` or/and "`symbol`", and which MAY have the member `"id"`. If given, the value of `"symbol"` MUST either be a string of the symbolic notation of the unit, or an object with the members `"value"` and `"type"` where `"value"` is the symbolic unit notation and `"type"` references the unit serialization scheme that is used. `"type"` MUST HAVE the value `"http://www.opengis.net/def/uom/UCUM/`" if [UCUM](http://unitsofmeasure.org) is used, or a custom value as recommended in section "Extensions". If given, the value of `"label"` MUST be an i18n object of the name of the unit and SHOULD be short. If given, the value of `"id"` MUST be a string and SHOULD be a common identifier. It is RECOMMENDED to reference a unit serialization scheme to allow automatic unit conversion.
*   A parameter object MUST NOT have a `"unit"` member if the `"observedProperty"` member has a `"categories"` member.

### 4.11 Metadata objects

The metadata object contains information about when the information was created and how it is licenced

*   A metadata object MUST have a member with the name `"terms"` value MUST be a string and SHOULD be a link to the terms and conditions for the use of the returned data.
*   A metadata object MUST have a member with the name `"date_created"` value MUST be a date string which follows the [RFC3339](https://tools.ietf.org/html/rfc3339) date format convention. This is the date time when the information returned was last updated (i.e. the model run time). The default representation SHOULD be UTC time plus LOCAL offset.
*   A metadata object SHOULD have a member with the name `"link"` value MUST be a string and MAY be a link to more documentation.
*   A metadata object SHOULD have a member with the name `"credit"` value MUST be a string and MAY be a link to any accreditation information that the user must display when using the product.
*   A metadata object MAY have a member with the name `"type"` value MUST be a string and be a name to describe the product.
*   A metadata object MAY have other members to provide extra information from the source provider.

### Appendix A. List of data types that MUST be supported by Feature Data compatible APIs

##### Air Temperature

Temperature value for the defined time

##### Wind Speed

Wind speed for the defined time

##### Wind Direction

Wind direction in decimal degrees for the defined time

##### Weather Text

Short Text description of weather conditions for the defined time

##### Weather Icon

Link to an icon describing weather conditions for the defined time

##### Maximum Air Temperature

Maximum Temperature value for the defined time period

##### Minimum Air Temperature

Minimum Temperature value for the defined time period

### Appendix B. Query options MUST be supported by Feature Data compatible APIs

#### Position of request

##### Query parameter to define location to return data for either :

###### Latitude

Latitude of data to request data for in decimal degrees

###### Longitude

Longitude of data to request data for in decimal degrees

##### Or (in order to simplify query caching) :

###### GeoHash

A GeoHash encoding of the latitude and longitude of the request

#### Units

Query parameter to define the type of units to return Metric or Imperial, by default Feature Data APIs should return data in Metric units

### Appendix C. Examples

##### i18n Objects

<pre class="language-json"><span class="token punctuation">{</span>
  <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"Temperature"</span><span class="token punctuation">,</span>
  <span class="token property">"de"</span><span class="token operator">:</span> <span class="token string">"Temperatur"</span><span class="token punctuation">,</span>
  <span class="token property">"no"</span><span class="token operator">:</span> <span class="token string">"Temperatur"</span><span class="token punctuation">,</span>
  <span class="token property">"fr"</span><span class="token operator">:</span> <span class="token string">"Température"</span>
<span class="token punctuation">}</span>
</pre>

##### time Objects

Example summary time object, with values valid for the period between 06Z on the 7th of September 2017 to 18Z on the 7th of September 2017.

<pre class="language-json"><span class="token punctuation">{</span>
    <span class="token property">"Max_Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">14</span><span class="token punctuation">,</span>
    <span class="token property">"Max_Air_Temperature"</span><span class="token operator">:</span> <span class="token number">18</span><span class="token punctuation">,</span>
    <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100/2017-09-07T18:00:00+0100"</span>
<span class="token punctuation">}</span>
</pre>

Example spot time object, with values valid for 06Z on the 7th of September 2017.

<pre class="language-json"><span class="token punctuation">{</span>
    <span class="token property">"UV_Index"</span><span class="token operator">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
    <span class="token property">"Wind_Speed"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
    <span class="token property">"Wind_Direction"</span><span class="token operator">:</span> <span class="token number">315</span><span class="token punctuation">,</span>
    <span class="token property">"Wind_Direction_Text"</span><span class="token operator">:</span> <span class="token string">"NW"</span><span class="token punctuation">,</span>    
    <span class="token property">"Weather_Type_Text"</span><span class="token operator">:</span> <span class="token string">"Medium-level cloud"</span><span class="token punctuation">,</span>
    <span class="token property">"Wind_Gust"</span><span class="token operator">:</span> <span class="token number">9</span><span class="token punctuation">,</span>
    <span class="token property">"Weather_Type"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
    <span class="token property">"Probability_of_Precipitation"</span><span class="token operator">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
    <span class="token property">"Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">12</span><span class="token punctuation">,</span>
    <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token number">15</span><span class="token punctuation">,</span>
    <span class="token property">"Weather_icon"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/webfiles/1500891453615/images/icons/wx/7.svg"</span><span class="token punctuation">,</span>
    <span class="token property">"Visibility"</span><span class="token operator">:</span> <span class="token string">"Very good"</span><span class="token punctuation">,</span>
    <span class="token property">"Relative_Humidity"</span><span class="token operator">:</span> <span class="token number">82</span><span class="token punctuation">,</span>
    <span class="token property">"UV_Index_text"</span><span class="token operator">:</span> <span class="token string">"Low"</span><span class="token punctuation">,</span>
    <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100"</span>
<span class="token punctuation">}</span>
</pre>

##### bbox Objects

Example of a 2D bbox member on a FeatureCollection:

<pre class="language-JSON"><span class="token punctuation">{</span>
    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"FeatureCollection"</span><span class="token punctuation">,</span>
    <span class="token property">"bbox"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token number">100.0</span><span class="token punctuation">,</span> <span class="token number">0.0</span><span class="token punctuation">,</span> <span class="token number">105.0</span><span class="token punctuation">,</span> <span class="token number">1.0</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"features"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
    //...
    <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</pre>

Example of a 3D bbox member with a depth of 100 meters:

<pre class="language-JSON"><span class="token punctuation">{</span>
    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"FeatureCollection"</span><span class="token punctuation">,</span>
    <span class="token property">"bbox"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token number">100.0</span><span class="token punctuation">,</span> <span class="token number">0.0</span><span class="token punctuation">,</span> -<span class="token number">100.0</span><span class="token punctuation">,</span> <span class="token number">105.0</span><span class="token punctuation">,</span> <span class="token number">1.0</span><span class="token punctuation">,</span> <span class="token number">0.0</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"features"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
    //...
    <span class="token punctuation">]</span>
<span class="token punctuation">}</span>   
</pre>

##### Day Objects

<pre class="language-json"><span class="token punctuation">{</span> 
    <span class="token property">"WxDate"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T01:00:00+0100"</span><span class="token punctuation">,</span>
    <span class="token property">"Sunrise"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:50:00+0100"</span><span class="token punctuation">,</span>
    <span class="token property">"Sunset"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T19:55:00+0100"</span><span class="token punctuation">,</span>
    <span class="token property">"Moonphase"</span><span class="token operator">:</span> <span class="token string">"Full moon"</span><span class="token punctuation">,</span>
    <span class="token property">"Times"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"Max_Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">14</span><span class="token punctuation">,</span>
            <span class="token property">"Max_Air_Temperature"</span><span class="token operator">:</span> <span class="token number">18</span><span class="token punctuation">,</span>
            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100/2017-09-07T18:00:00+0100"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>        
        <span class="token punctuation">{</span>
            <span class="token property">"UV_Index"</span><span class="token operator">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Speed"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Direction"</span><span class="token operator">:</span> <span class="token number">315</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Direction_Text"</span><span class="token operator">:</span> <span class="token string">"NW"</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_Type_Text"</span><span class="token operator">:</span> <span class="token string">"Medium-level cloud"</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Gust"</span><span class="token operator">:</span> <span class="token number">9</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_Type"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
            <span class="token property">"Probability_of_Precipitation"</span><span class="token operator">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
            <span class="token property">"Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">12</span><span class="token punctuation">,</span>
            <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token number">15</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_icon"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/webfiles/1500891453615/images/icons/wx/7.svg"</span><span class="token punctuation">,</span>
            <span class="token property">"Visibility"</span><span class="token operator">:</span> <span class="token string">"Very good"</span><span class="token punctuation">,</span>
            <span class="token property">"Relative_Humidity"</span><span class="token operator">:</span> <span class="token number">82</span><span class="token punctuation">,</span>
            <span class="token property">"UV_Index_text"</span><span class="token operator">:</span> <span class="token string">"Low"</span><span class="token punctuation">,</span>
            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>        
        <span class="token punctuation">{</span>
            <span class="token property">"UV_Index"</span><span class="token operator">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Speed"</span><span class="token operator">:</span> <span class="token number">9</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Direction"</span><span class="token operator">:</span> <span class="token number">315</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Direction_Text"</span><span class="token operator">:</span> <span class="token string">"NW"</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_Type_Text"</span><span class="token operator">:</span> <span class="token string">"Medium-level cloud"</span><span class="token punctuation">,</span>
            <span class="token property">"Wind_Gust"</span><span class="token operator">:</span> <span class="token number">12</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_Type"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
            <span class="token property">"Probability_of_Precipitation"</span><span class="token operator">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
            <span class="token property">"Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">11</span><span class="token punctuation">,</span>
            <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token number">15</span><span class="token punctuation">,</span>
            <span class="token property">"Weather_icon"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/webfiles/1500891453615/images/icons/wx/7.svg"</span><span class="token punctuation">,</span>
            <span class="token property">"Visibility"</span><span class="token operator">:</span> <span class="token string">"Very good"</span><span class="token punctuation">,</span>
            <span class="token property">"Relative_Humidity"</span><span class="token operator">:</span> <span class="token number">80</span><span class="token punctuation">,</span>
            <span class="token property">"UV_Index_text"</span><span class="token operator">:</span> <span class="token string">"Low"</span><span class="token punctuation">,</span>
            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T07:00:00+0100"</span>
        <span class="token punctuation">}</span>
        ...
    <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</pre>

##### Parameter objects

<pre class="language-json"><span class="token punctuation">{</span>
  <span class="token property">"type"</span> <span class="token operator">:</span> <span class="token string">"Parameter"</span><span class="token punctuation">,</span>
  <span class="token property">"description"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
    <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"The sea surface temperature in degrees Celsius."</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token property">"observedProperty"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
    <span class="token property">"id"</span> <span class="token operator">:</span> <span class="token string">"http://vocab.nerc.ac.uk/standard_name/sea_surface_temperature/"</span><span class="token punctuation">,</span>
    <span class="token property">"label"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
      <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"Sea Surface Temperature"</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token property">"description"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
      <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"The temperature of sea water near the surface (including the part under sea-ice, if any), and not the skin temperature."</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token property">"unit"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
    <span class="token property">"label"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
      <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"Degree Celsius"</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token property">"symbol"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
      <span class="token property">"value"</span><span class="token operator">:</span> <span class="token string">"Cel"</span><span class="token punctuation">,</span>
      <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"http://www.opengis.net/def/uom/UCUM/"</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</pre>

##### metadata object

Example for data produced by Norwegian Meteorological Institute

<pre class="language-json">    <span class="token property">"metadata"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
        <span class="token property">"date_created"</span><span class="token operator">:</span> <span class="token string">"2017-12-01T10:00:00+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"terms"</span><span class="token operator">:</span> <span class="token string">"https://www.met.no/en/free-meteorological-data/Licensing-and-crediting"</span><span class="token punctuation">,</span>
        <span class="token property">"Start date"</span><span class="token operator">:</span> <span class="token string">"2017-12-01T07:00:00+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"Next model"</span><span class="token operator">:</span> <span class="token string">"2017-12-01T10:00:00+0100"</span><span class="token punctuation">,</span>        
        <span class="token property">"End date"</span><span class="token operator">:</span> <span class="token string">"2017-12-03T18:00:00+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"link"</span><span class="token operator">:</span> <span class="token string">"https://www.met.no/en"</span><span class="token punctuation">,</span>
        <span class="token property">"model"</span><span class="token operator">:</span> <span class="token string">"LOCAL"</span><span class="token punctuation">,</span>
        <span class="token property">"Model run ended"</span><span class="token operator">:</span> <span class="token string">"2017-12-01T02:27:46+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"credit"</span><span class="token operator">:</span> <span class="token string">"Weather forecast from the Norwegian Meteorological Institute"</span>
    <span class="token punctuation">}</span>
</pre>

Example for data produced by the Met Office

<pre class="language-json">    <span class="token property">"metadata"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
        <span class="token property">"date_created"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T10:00:00+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"terms"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/datapoint/terms-conditions"</span><span class="token punctuation">,</span>
        <span class="token property">"link"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/"</span><span class="token punctuation">,</span>
        <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"UK Forecast data"</span><span class="token punctuation">,</span>
        <span class="token property">"credit"</span><span class="token operator">:</span> <span class="token string">"Weather forecast from the Met Office"</span>
    <span class="token punctuation">}</span>
</pre>

##### Result

<pre class="language-JSON"><span class="token punctuation">{</span>
    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"FeatureCollection"</span><span class="token punctuation">,</span>
    <span class="token property">"features"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"geometry"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
                <span class="token property">"coordinates"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
                    -<span class="token number">6.301</span><span class="token punctuation">,</span>
                    <span class="token number">49.913</span><span class="token punctuation">,</span>
                    <span class="token number">31</span>
                <span class="token punctuation">]</span><span class="token punctuation">,</span>
                <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"Point"</span>
            <span class="token punctuation">}</span><span class="token punctuation">,</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"3803"</span><span class="token punctuation">,</span>
            <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"Feature"</span><span class="token punctuation">,</span>
            <span class="token property">"properties"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
                <span class="token property">"Day"</span><span class="token operator">:</span><span class="token punctuation">[</span><span class="token punctuation">{</span> 
                    <span class="token property">"WxDate"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T01:00:00+0100"</span><span class="token punctuation">,</span>
                    <span class="token property">"Sunrise"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:50:00+0100"</span><span class="token punctuation">,</span>
                    <span class="token property">"Sunset"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T19:55:00+0100"</span><span class="token punctuation">,</span>
                    <span class="token property">"Moonphase"</span><span class="token operator">:</span> <span class="token string">"Full moon"</span><span class="token punctuation">,</span>
                    <span class="token property">"Times"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
                        <span class="token punctuation">{</span>
                            <span class="token property">"Max_Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">14</span><span class="token punctuation">,</span>
                            <span class="token property">"Max_Air_Temperature"</span><span class="token operator">:</span> <span class="token number">18</span><span class="token punctuation">,</span>
                            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100/2017-09-07T18:00:00+0100"</span>
                        <span class="token punctuation">}</span><span class="token punctuation">,</span>        
                        <span class="token punctuation">{</span>
                            <span class="token property">"UV_Index"</span><span class="token operator">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Speed"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Direction"</span><span class="token operator">:</span> <span class="token number">315</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Direction_Text"</span><span class="token operator">:</span> <span class="token string">"NW"</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_Type_Text"</span><span class="token operator">:</span> <span class="token string">"Medium-level cloud"</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Gust"</span><span class="token operator">:</span> <span class="token number">9</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_Type"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
                            <span class="token property">"Probability_of_Precipitation"</span><span class="token operator">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
                            <span class="token property">"Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">12</span><span class="token punctuation">,</span>
                            <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token number">15</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_icon"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/webfiles/1500891453615/images/icons/wx/7.svg"</span><span class="token punctuation">,</span>
                            <span class="token property">"Visibility"</span><span class="token operator">:</span> <span class="token string">"Very good"</span><span class="token punctuation">,</span>
                            <span class="token property">"Relative_Humidity"</span><span class="token operator">:</span> <span class="token number">82</span><span class="token punctuation">,</span>
                            <span class="token property">"UV_Index_text"</span><span class="token operator">:</span> <span class="token string">"Low"</span><span class="token punctuation">,</span>
                            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T06:00:00+0100"</span>
                        <span class="token punctuation">}</span><span class="token punctuation">,</span>        
                        <span class="token punctuation">{</span>
                            <span class="token property">"UV_Index"</span><span class="token operator">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Speed"</span><span class="token operator">:</span> <span class="token number">9</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Direction"</span><span class="token operator">:</span> <span class="token number">315</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Direction_Text"</span><span class="token operator">:</span> <span class="token string">"NW"</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_Type_Text"</span><span class="token operator">:</span> <span class="token string">"Medium-level cloud"</span><span class="token punctuation">,</span>
                            <span class="token property">"Wind_Gust"</span><span class="token operator">:</span> <span class="token number">12</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_Type"</span><span class="token operator">:</span> <span class="token number">7</span><span class="token punctuation">,</span>
                            <span class="token property">"Probability_of_Precipitation"</span><span class="token operator">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
                            <span class="token property">"Feels_Like_Temperature"</span><span class="token operator">:</span> <span class="token number">11</span><span class="token punctuation">,</span>
                            <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token number">15</span><span class="token punctuation">,</span>
                            <span class="token property">"Weather_icon"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/webfiles/1500891453615/images/icons/wx/7.svg"</span><span class="token punctuation">,</span>
                            <span class="token property">"Visibility"</span><span class="token operator">:</span> <span class="token string">"Very good"</span><span class="token punctuation">,</span>
                            <span class="token property">"Relative_Humidity"</span><span class="token operator">:</span> <span class="token number">80</span><span class="token punctuation">,</span>
                            <span class="token property">"UV_Index_text"</span><span class="token operator">:</span> <span class="token string">"Low"</span><span class="token punctuation">,</span>
                            <span class="token property">"valid_time"</span><span class="token operator">:</span> <span class="token string">"2017-09-07T07:00:00+0100"</span>
                        <span class="token punctuation">}</span>
                        ...
                    <span class="token punctuation">]</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                ...
                <span class="token punctuation">]</span><span class="token punctuation">,</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        ...
        <span class="token punctuation">{</span><span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"bbox"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        -<span class="token number">6.301</span><span class="token punctuation">,</span>
        <span class="token number">49.913</span><span class="token punctuation">,</span>
        -<span class="token number">6.301</span><span class="token punctuation">,</span>
        <span class="token number">49.913</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>    
    <span class="token property">"parameters"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"Air_Temperature"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
                <span class="token property">"type"</span> <span class="token operator">:</span> <span class="token string">"Parameter"</span><span class="token punctuation">,</span>
                <span class="token property">"description"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"The air temperature measured in degrees Celsius."</span><span class="token punctuation">,</span>
                    <span class="token property">"no"</span><span class="token operator">:</span> <span class="token string">"Lufttemperaturen målt i grader Celsius."</span><span class="token punctuation">,</span>
                    <span class="token property">"fr"</span><span class="token operator">:</span> <span class="token string">"La température de l'air mesurée en degrés Celsius."</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token property">"unit"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"label"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"Degree Celsius"</span><span class="token punctuation">,</span>
                    <span class="token property">"no"</span><span class="token operator">:</span> <span class="token string">"Grader celsius"</span><span class="token punctuation">,</span>
                    <span class="token property">"fr"</span><span class="token operator">:</span> <span class="token string">"Degré Celsius"</span>
                    <span class="token punctuation">}</span><span class="token punctuation">,</span>
                    <span class="token property">"symbol"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"value"</span><span class="token operator">:</span> <span class="token string">"Cel"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"http://www.opengis.net/def/uom/UCUM/"</span>
                    <span class="token punctuation">}</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token property">"observedProperty"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"id"</span> <span class="token operator">:</span> <span class="token string">"http://vocab.nerc.ac.uk/standard_name/air_temperature/"</span><span class="token punctuation">,</span>
                    <span class="token property">"label"</span> <span class="token operator">:</span> <span class="token punctuation">{</span>
                    <span class="token property">"en"</span><span class="token operator">:</span> <span class="token string">"Air temperature"</span><span class="token punctuation">,</span>
                    <span class="token property">"no"</span><span class="token operator">:</span> <span class="token string">"Lufttemperatur"</span><span class="token punctuation">,</span>
                    <span class="token property">"fr"</span><span class="token operator">:</span> <span class="token string">"Température de l'air"</span>
                    <span class="token punctuation">}</span>
                <span class="token punctuation">}</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        ...
        <span class="token punctuation">{</span><span class="token punctuation">}</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token property">"metadata"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
        <span class="token property">"terms"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/datapoint/terms-conditions"</span><span class="token punctuation">,</span>
        <span class="token property">"date_created"</span><span class="token operator">:</span> <span class="token string">"2017-09-05T10:00:00+0100"</span><span class="token punctuation">,</span>
        <span class="token property">"link"</span><span class="token operator">:</span> <span class="token string">"http://www.metoffice.gov.uk/"</span><span class="token punctuation">,</span>
        <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"UK Forecast data"</span><span class="token punctuation">,</span>
        <span class="token property">"credit"</span><span class="token operator">:</span> <span class="token string">"Weather forecast from the Met Office"</span>
    <span class="token punctuation">}</span>        
    <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</pre>

### Attribution

This specification uses ideas and sentence structures from the [GeoJSON](https://tools.ietf.org/html/rfc7946 "GeoJSON") specification which is licensed under a [Creative Commons Attribution 3.0 United States License](https://creativecommons.org/licenses/by/3.0/us/) and the [CoverageJSON](https://github.com/covjson/specification/blob/master/spec.md "CoverageJSON") specification which is licensed under [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

</div>

<div class="md-sidebar-toc">

*   [Weather on the web Feature data format proposal](#weather-on-the-web-feature-data-format-proposal)
    *   [1 Abstract](#1-abstract)
    *   [2 Introduction](#2-introduction)
    *   [3 Requirements language](#3-requirements-language)
    *   [4 Data format](#4-data-format)
        *   [4.1 Definitions](#41-definitions)
        *   [4.2 i18n Objects](#42-i18n-objects)
        *   [4.3 Type Object](#43-type-object)
        *   [4.4 Features Array](#44-features-array)
        *   [4.5 Feature Object](#45-feature-object)
        *   [4.6 Properties Object](#46-properties-object)
        *   [4.7 Day Object](#47-day-object)
        *   [4.8 Time Object](#48-time-object)
        *   [4.9 bbox Object](#49-bbox-object)
        *   [4.10 Parameter Objects](#410-parameter-objects)
        *   [4.11 Metadata objects](#411-metadata-objects)
        *   [Appendix A. List of data types that MUST be supported by Feature Data compatible APIs](#appendix-a-list-of-data-types-that-must-be-supported-by-open-weather-compatible-apis)
        *   [Appendix B. Query options MUST be supported by Feature Data compatible APIs](#appendix-b-query-options-must-be-supported-by-open-weather-compatible-apis)
            *   [Position of request](#position-of-request)
                *   [Query parameter to define location to return data for either :](#query-parameter-to-define-location-to-return-data-for-either)
                *   [Or (in order to simplify query caching) :](#or-in-order-to-simplify-query-caching)
            *   [Units](#units)
        *   [Appendix C. Examples](#appendix-c-examples)  
            * [i18n Objects](#i18n-objects)  
            * [time Objects](#time-objects)  
            * [bbox Objects](#bbox-objects)  
            * [Day Objects](#day-objects)  
            * [Parameter objects](#parameter-objects)  
            * [metadata object](#metadata-object)  
            * [Result](#result)
        *   [Attribution](#attribution)

</div>
