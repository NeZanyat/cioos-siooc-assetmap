# Assetmap starting point

Containt basic html and javascript to be embedded in a HTML page.

This isn't the final project nor the Wordpress plugin.

Use Flask to enable a simple proxy to test CKAN API access and serve static file.

# Installation

Tested with python 3.7 on windows 10

* Create virtual environment for the project
    * python -m venv path/to/project/venv
* activate environment
    * path/to/project/venv/Script/activate
* install dependencies
    * pip install -r requirements.txt
* Execute Flask server
    * python main.py
    
# Docker installation

```
­> git clone <project_url>
> cd <root_of_project>
> docker-compose up
```

# Configuration files

## ckan.json
 
Link to the CKAN instance:

* base_url : Homepage of the CKAN instance to redirect user to
* api_url : API URL to access the desired CKAN instance. This is use by the proxy server
* dataset_url : url to follow to direct a user to the dataset page of the ckan instance
* organization_url : url to follow to direct a user to the organization page of the ckan instance
* access_url : url to use to access the API. /ckan/ is the default proxy
* add_language_url : Boolean to enable the desired language to be displayed by CKAN. The language code is added after url be before the id
* start_bbox: Initial bounding box to restrict search to

Example of the configuration to use open.canada.ca catalog
```
{
    "base_url":"http://open.canada.ca/",
    "api_url":"http://open.canada.ca/data/api/",
    "dataset_url": "http://open.canada.ca/data/",
    "organization_url": "http://open.canada.ca/data/",
    "access_url":"/ckan/",
    "add_language_url": true,
    "start_bbox": [-118, 36, -40, 63]
}
```

## filters.json

All categories, variables and CKAN search criteria.

Categories contains a list of variable. Either element has the transalted lable, icon and if the item is enabled or not. Each category is reprensented by a clickable icon at the top left corner. All the variables of a category are grouped inside a panel. This panel is shown when the category icon is click. 

```
{
    categories: [{
        "id": "physic",
        "label":
        {
            "fr": "PHYSIQUE",
            "en": "PHYSICS"
        },
        "icon": "physics.png",
        "enabled": true,
        "variables":[{}]
    }]
}

```

Each variable defines the text used in the ckan package seach. This is a temporary solution until the search criteria and schema definition is established. The same "ckantext" is used to map the variable to a dataset via the tag/keyword. This is still a temporary solution. The translation of the search term might be necessary if no tranlsation in the ckan dataset is available. The icon file are in /images and the thumbnail in /images/thumbnails. As of now, the is no translation for the icon is planned.

```
    "variables":[{
        "id": "var_salinity",
        "icon":"seaWaterSalinity.png",
        "label":
        {
            "fr":"Salinité",
            "en":"Salinity"
        },
        "ckantext":"salinity",
        "enabled": true
    }]
```

## map.json

Display style and initial map attributes.

start-view is mapped to the ol.view use by the openlayer map. the center element is in WGS84( ESPG:4326 ) and transform to the correct map projection system during creation.
```
"start_view": {
        "zoom": 4,
        "center": [-68, 48]
    }
```

The Polystyle is used for the display of rectangular extent. Stroke is used for the outer rectangle region and Fill is for the rectangle interior. Those element are use as is in the vector layor style creation. Other item can be added is supported by openlayer. 
```
"Polystyle":{
        "Stroke":{
            "color": "blue",
            "lineDash": [4],
            "width": 3
        },
        "Fill":{
            "color": "rgba(0, 0, 255, 0.1)"
        }
    }
```
Back layer configuration still to implement.

## ui_str.json

Translation strings.

List of strings defined by an name and the different version per language supported. 
```
"dataset_title": {
    "fr": "Titre du jeu de données",
    "en": "Dataset title"
  }
```

The class StringTranslator use thisconfiguration and can has method to retrieve the correct string for a specified language or the one set as current. 

# References

[Mockup](https://xd.adobe.com/view/f27999f2-a6d1-4498-51b1-37dc757286ff-8448/screen/6b487dff-190c-45b1-b1ca-478295611337/Web-1920-15/)

# Quick Todo:


* better code documentation
* Multiple varaible selection should restrict not open up search ( and vs or )
* Change current icon for new one ( currently canibalised from Condition Maritime )
* generate ckan dataset/organisation url with language ( if supported )
* bettter ids for dataset panel ( limit conflict with crm )
* on detail click, scroll to correct panel
* add CSS for variable label et category label + layout
* position variable with 3 per row
* sort rectangle by area size
* transform CKAN description in html ( markdown style)
* hide/show variable panel drawer
* hide/show dataset details drawer
* show the map control and attribution even with open panel
* add responsive css
* better error checking
    * Look if dataset has a minimum ( id, title, etc ) before adding it
* CSS, again and again and again...

# Done

* generate tab panel per category
* ON detail panel click "Map", select dataset on the map
* add bbox region to search function
    * only in ckan config for now
    * write search url wih bbox as filter
* config file documentation
* transfrom center view to use WGS84 lat/long in config
* on details click, open dataset details 
* generate icon with multiple size ( thumbnail )
* look for tag to add variable icon in detail of dataset  ( used keyword/tags with variable ckantext )
* iterate through dataset resources for other view tool links ( simulate tool with PDF, WMS, CSV )
* remove methode reveiving language in parameter and rely only on i18n class
* Docker image for test deployment
* Clean all if no variable selected 
* multiple call to retrieve all data ( paginated, not just first 10)
* json for translation ( fr, en, other?)
* encapsulate translation in a class that know the current langage ( getUISTR, default if not found )
* load config for proxy in ckan.json in python
* add config for initial map display
    * zoom level
    * extent
    * rectangle color/fill/outset 
    * multiple section vector type ( only rectangle for now ) 
