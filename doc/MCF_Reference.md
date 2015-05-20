# Metadata Control File Reference

## Table of Contents
* [Basic Concepts](#basic-concepts)
* [Nesting MCF Files](#nesting-mcf-files)
* [Encoding](#encoding)
* [Keyword Substitution](#keyword-substitution)
* [Sections](#sections)
  * [Metadata](#metadata)
  * [Spatial](#spatial)
  * [Identification](#identification)
  * [Contact:main](#contact&#58;main)
  * [Contact:distribution](#contact&#58;distribution)
  * [Contact:*](#contact&#58;*)

## Basic Concepts

* Sections are case insensitive
* Section parameters are case insensitive
* Section parameter values are case sensitive
* If an optional section is specified, then its child parameters' cardinality are enforced
* Filename conventions are up to the user. However, below are some suggestions:
 * use ``.mcf`` as file extension
 * name the MCF file basename the same as the dataset (e.g. ``foo.shp``, ``foo.mcf``)
* To add a comment in an MCF file, a line that begins with a hash tag (``#``) will be ignored

## Nesting MCF files

In the case the user is generating metadata for multiple datasets which have common information, it becomes efficient to nest MCF files together. pygeometa allows chaining MCF files to inherit values from other MCF files. Example: multiple datasets MCF files can refer a single MCF file that contain contact information common to all those datasets.

To use MCF file nesting:
* At the top of the [metadata] section of an MCF file add ``base_mcf=foo.mcf``

Notes about nesting MCF files: 
* You can refer to one and only one ``base_mcf`` in a MCF file
* When a parameter is defined in both the base_mcf file and the current MCF file, it's always the current MCF file that overwrites the base_mcf file
* MCF files can be nested in chains, meaning a MCF file can be used by a 'child' MCF file and use a 'parent' MCF file
* The base_mcf file must be in the same folder than the current MCF file


## Encoding

The MCF must be utf8 encoding.

Is your MCF Encoded as UTF8?

```bash
# editing in vim
# :set encoding=utf8
# pasting in vim
# in insert mode, hit CTRL-V $CODE, where $CODE is as per http://www.htmlhelp.com/reference/charset
# to see how the file is actually encoded on disk
file --mime-encoding file.txt
file -i file.txt
# to convert from one encoding to another
iconv -f iso8859-1 -t utf-8 file.mcf > file.mcf.new
```

## Keyword Substitution

If you're using keyword substitution for some values such as $Date$, Subversion files need to have keyword substitution enabled. To enable the $Date$ keyword substitution on sample.mcf:
```
svn propset svn:keywords "Date" sample.mcf
```
You need to run this command once per new MCF file which has the $Date$ keyword.


## Sections

### `[metadata]`

Property Name|Mandatory/Optional|Description|Example|Reference
-------------|------------------|-----------|-------|---------:
identifier|Mandatory|unique identifier for this metadata file|11800c2c-e6b9-11df-b9ae-0014c2c00eab|ISO 19115:2003 Section B.2.1
language|Mandatory|primary language used for documenting metadata, the metadata records themselves can be provided in multiple languages nonetheless|eng; CAN|ISO 19115:2003 Section B.2.1
charset|Mandatory|full name of the character coding standard used for the metadata set|utf8|ISO 19115:2003 Section B.2.1
parentidentifier|Optional|file identifier of the metadata to which this metadata is a subset|11800c2c-e6b9-11df-b9ae-0014c2c33ebe|ISO 19115:2003 Section B.2.1
hierarchylevel|Mandatory|level to which the metadata applies (must be one of 'series', 'software', 'featureType', 'model', 'collectionHardware', 'collectionSession', 'nonGeographicDataset', 'propertyType', 'fieldSession', 'dataset', 'service', 'attribute', 'attributeType', 'tile', 'feature', 'dimensionGroup'|dataset|ISO 19115:2003 Section B.2.1
datestamp|Mandatory|date that the metadata was created, pygeometa supports specifying the $Date$ variable to have this value updated at every Subversion commit |2000-11-11 or 2000-01-12T11:11:11Z|ISO 19115:2003 Section B.2.1
dataseturi|Mandatory|Uniformed Resource Identifier (URI) of the dataset to which the metadata applies|`urn:x-wmo:md:int.wmo.wis::http://geo.woudc.org/def/data/uv-radiation/uv-irradiance`|ISO 19115:2003 Section B.2.1

### `[spatial]`

Property Name|Mandatory/Optional|Description|Example|Reference
-------------|------------------|-----------|-------|---------:
datatype|Mandatory|method used to represent geographic information in the dataset (must be one of 'vector', 'grid', 'textTable', 'tin', 'stereoModel', 'video')|vector|Section B.5.26
geomtype|Mandatory|name of point or vector objects used to locate zero-, one-, two-, or threedimensional spatial locations in the dataset (must be one of 'complex', 'composite', 'curve', 'point', 'solid', 'surface')|point|ISO 19115:2003 B.5.15
crs|Mandatory|EPSG code identifier|4326|ISO 19115:2003 B.2.7.3
bbox|Mandatory|geographic position of the dataset, formatted as 'minx,miny,maxx,maxy'|-141,42,-52,84|ISO 19115:2003 Section B.3.1.2


### `[identification]`

Property Name|Mandatory/Optional|Description|Example|Reference
-------------|------------------|-----------|-------|---------:
language|Mandatory|language(s) used within the dataset|eng; CAN|ISO 19115:2003 Section B.2.2.1
charset|Mandatory|full name of the character coding standard used for the dataset|eng; CAN|ISO 19115:2003 Section B.2.1
title_en|Mandatory|name by which the cited resource is known (English)|Important Bird Areas|ISO 19115:2003 Section B.3.2.1
title_fr|Mandatory|name by which the cited resource is known (French)|Zone importante d'oiseau|ISO 19115:2003 Section B.3.2.1
abstract_en|Mandatory|brief narrative summary of the content of the resource(s) (English)|Birds in important areas...|ISO 19115:2003 Section B.2.2.1
abstract_fr|Mandatory|brief narrative summary of the content of the resource(s) (French)|Birds in important areas...|ISO 19115:2003 Section B.2.2.1
keywords_gc_cst_en|Optional|category keywords (English), HNAP requires a minimum of 1 keyword from the GoC Core Subject Thesaurus|keyword1,keyword2,keyword3|ISO 19115:2003 Section B.2.2.1
keywords_gc_cst_fr|Optional|category keywords (French), HNAP requires a minimum of 1 keyword from the GoC Core Subject Thesaurus|keyword1,keyword2,keyword3|ISO 19115:2003 Section B.2.2.1
keywords_en|Mandatory|category keywords (English), becomes Optional if keywords_gc_cst_en is provided|keyword1,keyword2,keyword3|ISO 19115:2003 Section B.2.2.1
keywords_fr|Mandatory|category keywords (French), becomes Optional if keywords_gc_cst_fr is provided|keyword1,keyword2,keyword3|ISO 19115:2003 Section B.2.2.1
keywords_type|Mandatory|subject matter used to group similar keywords (must be one of 'discipline', 'place', 'stratum', 'temporal', 'theme')|theme|ISO 19115:2003 Section B.2.2.3
topiccategory|Mandatory|main theme(s) of the dataset (must be one of 'geoscientificInformation', 'farming', 'elevation', 'utilitiesCommunication', 'oceans', 'boundaries', 'inlandWaters', 'intelligenceMilitary', 'environment', 'location', 'economy', 'planningCadastre','biota', 'health', 'imageryBaseMapsEarthCover', 'transportation', 'society', 'structure', 'climatologyMeteorologyAtmosphere'|climatologyMeteorologyAtmosphere|ISO 19115:2003 Section B.5.27
date|Mandatory|reference date for the cited resource|2000-09-01 or 2000-09-01T00:00:00Z|ISO 19115:2003 Section B.3.2.4
datetype|Mandatory|date used for the reference date (must be one of 'revision', 'publication', 'creation')|creation|ISO 19115:2003 Section B.3.2.4
fees|Mandatory|fees and terms for retreiving the resource.  Include monetary units (as specified in ISO 4217).  If there are no fees, use the term 'None'|None,ISO 19115:2003 Section B.2.10.6
accessconstraints|Mandatory|access constraints applied to assure the protection of privacy or intellectual property, and any special restrictions or limitations on obtaining the resource or metadata (must be one of 'patent', 'otherRestrictions','copyright','trademark', 'patentPending','restricted','license', 'intellectualPropertyRights').  If there are no accessconstraints, use the term 'otherRestrictions'|None|ISO 19115:2003 Section B.2.3
rights_en|Mandatory|Information about rights held in and over the resource (English). The $Date$ variable instead of yearvalue to update to current year at Subversion commit time. |Copyright (c) 2010 Her Majesty the Queen in Right of Canada|DMCI 1.1
rights_fr|Mandatory|Information about rights held in and over the resource (French). The $Date$ variable instead of yearvalue to update to current year at Subversion commit time. |Copyright (c) 2010 Her Majesty the Queen in Right of Canada|DMCI 1.1
url|Mandatory|URL of the dataset to which the metadata applies|http://host/path/|ISO 19115:2003 Section B.2.1
temporal_begin|Mandatory|Starting time period covered by the content of the dataset, either time period (startdate/enddate) or a single point in time value|1950-07-31|ISO 19115:2003 Section B.3.1.3
temporal_end|Mandatory|End time period covered by the content of the dataset, either time period (startdate/enddate) or a single point in time value.  For data updated in realtime, use the term `now`|now|ISO 19115:2003 Section B.3.1.3
status|Mandatory|"the status of the resource(s) (must be one of 'planned','historicalArchive','completed','onGoing', 'underDevelopment','required','obsolete')",completed,ISO 19115:2003 Section B.2.2.1
maintenancefrequency|Mandatory|frequency with which modifications and deletions are made to the data after it is first produced (must be one of 'continual', 'daily', 'weekly', 'fortnightly', 'monthly', 'quarterly', 'biannually', 'annually', 'asNeeded', 'irregular', 'notPlanned', 'unknown'|continual|ISO 19115:2003 B.5.18


### `[contact:main]`

The `[contact:main]` section provides information for the `pointOfContact` role (see ISO 19115:2003 Section B.3.2.1).

Property Name|Mandatory/Optional|Description|Example|Reference
-------------|------------------|-----------|-------|---------:
organization|Mandatory|name of the responsible organization (English)|Environment Canada|ISO 19115:2003 Section B.3.2.1
organization_en|Mandatory|name of the responsible organization (English)|Environment Canada|Harmonized NAP
organization_fr|Mandatory|name of the responsible organization (French)|Environment Canada|Harmonzed NAP
url|Mandatory|on-line information that can be used to contact the individual or organization|http://host/path|ISO 19115:2003 Section B.3.2.3
individualname|Mandatory|name of the responsible person-surname|given name|title seperated by a delimiter|Kralidis, Tom|ISO 19115:2003 Section B.3.2.1
positionname|Mandatory|role or position of the responsible person|Senior Systems Scientist|ISO 19115:2003 Section B.3.2.1
phone|Mandatory|telephone number by which individuals can speak to the responsible organization or individual|+01-416-739-4907|ISO 19115:2003 Section B.3.2.7
fax|Mandatory|telephone number of a facsimile machine for the responsible organization or individual|+01-416-739-4261|ISO 19115:2003 Section B.3.2.7
address|Mandatory|address line for the location (as described in ISO 11180| Annex A)|4905 Dufferin Street, 4N204|ISO 19115:2003 Section B.3.2.2
city|Mandatory|city of the location|Toronto|ISO 19115:2003 Section B.3.2.2
administrativearea|Mandatory|state, province of the location|Ontario|ISO 19115:2003 Section B.3.2.2
postalcode|Mandatory|ZIP or other postal code|M3H 5T4|ISO 19115:2003 Section B.3.2.2
country|Mandatory|country of the physical address|Canada|ISO 19115:2003 Section B.3.2.2
email|Mandatory|address of the electronic mailbox of the responsible organization or individual|tom.kralidis@ec.gc.ca|ISO 19115:2003 Section B.3.2.2
hoursofservice|Optional|time period (including time zone) when individuals can contact the organization or individual|0700h - 1500h EST|ISO 19115:2003 Section B.3.2.3
contactinstructions|Optional|supplementalinstructions on how or when to contact the individual or organization|contact during working business hours|ISO 19115:2003 Section B.3.2.3

### `[contact:distribution]`

The `[contact:distribution]` section provides information for the `distributor` role (see ISO 19115:2003 Section B.3.2.1) and has the identical structure as `[contact:main]`.

If contact information is the same for both, specify only `ref=contact:main` to have it provided in both sections in the metadata.


### `[distribution:*]`

MCF files can have 1..n `[distribution:*]` sections as required (e.g. `[distribution:wms]`, `[distribution:waf]`, etc.).

Property Name|Mandatory/Optional|Description|Example|Reference
-------------|------------------|-----------|-------|---------:
url|Mandatory|location (address) for on-line access using a Uniform Resource Locator address or similar addressing scheme such as http://www.isotc211.org/|http://host/path|ISO 19115:2003 Section B.3.2.5
type|Mandatory|connection protocol to be used.  Must be one of the `identifier` values from https://github.com/OSGeo/Cat-Interop/blob/master/LinkPropertyLookupTable.csv|WWW:LINK|ISO 19115:2003 Section B.3.2.5
name_en|Mandatory|English name of the online resource|roads|ISO 19115:2003 Section B.3.2.5
name_fr|Mandatory|French name of the online resource|roads|ISO 19115:2003 Section B.3.2.5
description_en|Mandatory|detailed text description of what the online resources is/does (English)|Important Bird Areas|ISO 19115:2003 Section B.3.2.5
description_fr|Mandatory|detailed text description of what the online resources is/does (French)|Zone importante d'oiseau|ISO 19115:2003 Section B.3.2.5
function|Mandatory|code for function performed by the online resource (must be one of 'download', 'information', 'offlineAccess', 'order', 'search')||ISO 19115:2003 Section B.3.2.5