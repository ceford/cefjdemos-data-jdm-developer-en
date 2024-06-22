<!-- Filename: J4.x:Joomla_Core_APIs / Display title: Joomla Core APIs -->

Joomla!  4.0

This page lists the endpoints available in Joomla by example of curl
commands.

Every URL requires authentication unless designated a public URL. For
security in Joomla 4.0.0 we plan to make the default Api Application
require a Super User account (as the API Application is brand new), this
may get relaxed as the API stabilizes and is well tested in the
community. If you are using the basic authentication plugin (currently
the only shipped plugin as of Joomla 4 alpha 10) it requires the
addition to the curl commands below using --user user_name:password

Every URL needs to be prepended with the Joomla host before the path
(e.g. instead of `/api/index.php/v1/article` you need
`http://example.com/api/index.php/v1/article`)

Any names of properties in curly brackets ({}) indicate the property is
a variable that should be substituted.

Unless otherwise noted these APIs were introduced in Joomla 4. For more
information on the Joomla API Specification (as opposed to this list of
URLs and options) please visit the [Joomla Api
Specification](https://docs.joomla.org/Joomla_Api_Specification "Special:MyLanguage/Joomla Api Specification").

Where end points require the value of {app} the variable currently takes
two values, site (front end), or administrator {back end)

## Useful Resources

A number of complimentary resources have been created to introduce
Joomla Web Services and assist you with learning how to implement Web
Services on your project.

- <a href="https://github.com/alexandreelise/j4x-api-collection"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Postman collection of Joomla Web
  Services API calls by Alexandre Elise</a>
- <a href="https://www.youtube.com/watch?v=lT9qodsvfZg"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Youtube: Joomla 4 tutorial: Using the
  Web Services API with Eoin Oliver</a>
- <a
  href="https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Community Magazine: Joomla Web Services API 101 by Patrick Jackson</a>

## Banners

### Banners

#### Get List of Banners

curl -X GET /api/index.php/v1/banners

#### Get Single Banner

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Delete Banner

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Create Banner

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d

    {
        "catid": 3,
        "clicks": 0,
        "custombannercode": "",
        "description": "Text",
        "metakey": "",
        "name": "Name",
        "params": {
            "alt": "",
            "height": "",
            "imageurl": "",
            "width": ""
        }
    }

#### Update Banner

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

    {
        "alias": "name",
        "catid": 3,
        "description": "New Text",
        "name": "New Name"
    }

### Clients

#### Get List of Clients

curl -X GET /api/index.php/v1/banners/clients

#### Get Single Client

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Delete Client

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Create Client

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/clients -d

    {
        "contact": "Name",
        "email": "email@mail.com",
        "extrainfo": "",
        "metakey": "",
        "name": "Clients",
        "state": 1
    }

#### Update Client

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d

    {
        "contact": "new Name",
        "email": "newemail@mail.com",
        "name": "Clients"
    }

### Categories

#### Get List of Categories

curl -X GET /api/index.php/v1/banners/categories

#### Get Single Category

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Delete Category

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Create Category

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d

    {
        "access": 1,
        "alias": "cat",
        "extension": "com_banners",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "published": 1,
        "title": "Title"
    }

#### Update Category

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/categories/{category_id} -d

    {
        "alias": "cat",
        "note": "Some Text",
        "parent_id": 1,
        "title": "New Title"
    }

### Content History

#### Get List of Content Histories

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Toggle Keep Content History

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}

#### Delete Content History

curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Config

### Application

#### Get List of Application Configs

curl -X GET /api/index.php/v1/config/application

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "debug": true,
        "sitename": "123"
    }

### Component

#### Get List of Component Configs

curl -X GET /api/index.php/v1/config/{component_name}

Example “component_name” is “com_content”.

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "link_titles": 1
    }

## Contact

### Contact

#### Get List of Contacts

curl -X GET /api/index.php/v1/contact

#### Get Single Contact

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Delete Contact

curl -X DELETE /api/index.php/v1/contact/{contact_id}

#### Create Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d

    {
        "alias": "contact",
        "catid": 4,
        "language": "*",
        "name": "Contact"
    }

#### Update Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d

    {
        "alias": "contact",
        "catid": 4,
        "name": "New Contact"
    }

#### Submit Contact Form

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d

    {
        "contact_email": "email@mail.com",
        "contact_message": "some text",
        "contact_name": "name",
        "contact_subject": "subject"
    }

### Categories

1.  Route Contact Categories is: "v1/contact/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/").

### Fields Contact

#### Get List of Fields Contact

curl -X GET /api/index.php/v1/fields/contact/contact

#### Get Single Field Contact

curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}

#### Delete Field Contact

curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}

#### Create Field Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Update Field Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact/{field_id} -d

    {
        "title": "new contact field",
        "name": "contact-field",
        "label": "contact field",
        "default_value": "",
        "type": "text",
        "note": "",
        "description": "Some New Text"
    }

### Fields Contact Mail

1.  Route Fields Contact Mail is: "v1/fields/contact/mail"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Fields Contact Categories

1.  Route Fields Contact Categories is: "v1/fields/contact/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Contact

#### Get List of Groups Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact

#### Get Single Group Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Delete Group Fields Contact

curl -X DELETE
/api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Create Group Fields Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field3",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Update Group Fields Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d

    {
        "title": "new contact group",
        "note": "",
        "description": "new description"
    }

### Group Fields Contact Mail

1.  Route Group Fields Contact Mail is: "v1/fields/groups/contact/mail"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Group Fields Contact Categories

1.  Route Group Fields Contact Categories is:
    "v1/fields/groups/contact/categories"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Content History

1.  Route Content History is: "v1/contact/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Content

### Articles

#### Get List of Articles

curl -X GET /api/index.php/v1/content/articles

#### Get Single Article

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Delete Article

curl -X DELETE /api/index.php/v1/content/articles/{article_id}

#### Create Article

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d

    {
        "alias": "my-article",
        "articletext": "My text",
        "catid": 64,
        "language": "*",
        "metadesc": "",
        "metakey": "",
        "title": "Here's an article"
    }

Currently the options mentioned here are required properties. However
the intention is currently to make AT LEAST metakey and metadesc
optional in the API.

#### Update Article

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d

    {
        "catid": 64,
        "title": "Updated article"
    }

### Categories

1.  Route Content Categories is: "v1/content/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/"),
    note if workflows is enabled then specifying a workflow is required
    (similarly to the UI)

### Fields Articles

1.  Route Fields Articles is: "v1/fields/content/articles"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Articles

1.  Route Groups Fields Articles is: "v1/fields/groups/content/articles"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Fields Categories

1.  Route Fields Categories is: "v1/fields/groups/content/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Content History

1.  Route Content History is:
    "v1/content/articles/{article_id}/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Languages

### Languages

#### Get List of Languages

curl -X GET /api/index.php/v1/languages

#### Install Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages -d

    {
        "package": "pkg_fr-FR"
    }

### Content Languages

#### Get List of Content Languages

curl -X GET /api/index.php/v1/languages/content

#### Get Single Content Language

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Delete Content Language

curl -X DELETE /api/index.php/v1/languages/content/{language_id}

#### Create Content Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/content -d

    {
        "access": 1,
        "description": "",
        "image": "fr_FR",
        "lang_code": "fr-FR",
        "metadesc": "",
        "metakey": "",
        "ordering": 1,
        "published": 0,
        "sef": "fk",
        "sitename": "",
        "title": "French (FR)",
        "title_native": "Français (France)"
    }

#### Update Content Language

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/content/{language_id} -d

    {
        "description": "",
        "lang_code": "en-GB",
        "metadesc": "",
        "metakey": "",
        "sitename": "",
        "title": "English (en-GB)",
        "title_native": "English (United Kingdom)"
    }

### Overrides Languages

#### Get List of Overrides Languages Constants

curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}

#### Get Single Override Language Constant

curl -X GET
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Delete Content Language

curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Create Content Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d

    {
        "key":"new_key",
        "override": "text"
    }

#### Update Content Language

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d

    {
        "key":"new_key",
        "override": "new text"
    }

var app - enum {"site", "administrator"}

var lang_code - string Example: “fr-FR“, “en-GB“ you can get lang_code
from v1/languages/content

#### Search Override Constant

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d

    {
        "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
        "searchtype": "constant"
    }

var searchtype - enum {“constant”, “value”}. “constant” search by
constant name, “value” - search by constant value

#### Refresh Override Search Cache

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh

## Menus

### Menus

#### Get List of Menus

curl -X GET /api/index.php/v1/menus/{app}

#### Get Single Menu

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Delete Menu

curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}

#### Create Menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d

    {
        "client_id": 0,
        "description": "The menu for the site",
        "menutype": "menu",
        "title": "Menu"
    }

#### Update Menu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d

    {
        "menutype": "menu",
        "title": "New Menu"
    }

### Menus Items

#### Get List of Menus Items Types

curl -X GET /api/index.php/v1/menus/{app}/items/types

#### Get List of Menus Items

curl -X GET /api/index.php/v1/menus/{app}/items

#### Get Single Menu Item

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Delete Menu Item

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Create Menu Item

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

    {
        "access": "1",
        "alias": "",
        "associations": {
            "en-GB": "",
            "fr-FR": ""
        },
        "browserNav": "0",
        "component_id": "20",
        "home": "0",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "params": {
            "cancel_redirect_menuitem": "",
            "catid": "",
            "custom_cancel_redirect": "0",
            "enable_category": "0",
            "menu-anchor_css": "",
            "menu-anchor_title": "",
            "menu-meta_description": "",
            "menu-meta_keywords": "",
            "menu_image": "",
            "menu_image_css": "",
            "menu_show": "1",
            "menu_text": "1",
            "page_heading": "",
            "page_title": "",
            "pageclass_sfx": "",
            "redirect_menuitem": "",
            "robots": "",
            "show_page_heading": ""
        },
        "parent_id": "1",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "template_style_id": "0",
        "title": "title",
        "toggle_modules_assigned": "1",
        "toggle_modules_published": "1",
        "type": "component"
    }

Example for "Create Article Page"

#### Update Menu Item

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d

    {
        "component_id": "20",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "title": "new title",
        "type": "component"
    }

Example for "Create Article Page"

## Messages

### Messages

#### Get List of Messages

curl -X GET /api/index.php/v1/messages

#### Get Single Message

curl -X GET /api/index.php/v1/messages/{message_id}

#### Delete Message

curl -X DELETE /api/index.php/v1/messages/{message_id}

#### Create Message

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/messages -d

    {
        "message": "text",
        "state": 0,
        "subject": "text",
        "user_id_from": 773,
        "user_id_to": 772
    }

#### Update Message

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/messages/{message_id} -d

    {
        "message": "new text",
        "subject": "new text",
        "user_id_from": 773,
        "user_id_to": 772
    }

## Modules

### Modules

#### Get List of Modules Types

curl -X GET /api/index.php/v1/modules/types/{app}

#### Get List of Modules

curl -X GET /api/index.php/v1/modules/{app}

#### Get Single Module

curl -X GET /api/index.php/v1/modules/{app}/{module_id}

#### Delete Module

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Create Module

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d

    {
        "access": "1",
        "assigned": [
            "101",
            "105"
        ],
        "assignment": "0",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "params": {
            "bootstrap_size": "0",
            "cache": "1",
            "cache_time": "900",
            "cachemode": "static",
            "count": "10",
            "header_class": "",
            "header_tag": "h3",
            "layout": "_:default",
            "module_tag": "div",
            "moduleclass_sfx": "",
            "style": "0"
        },
        "position": "",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "showtitle": "1",
        "title": "Title"
    }

Example for "Articles - Archived"

#### Update Module

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d

    {
        "access": "1",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "title": "New Title"
    }

Example for "Articles - Archived"

## Newsfeeds

### Feeds

#### Get List of Feeds

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Get Single Feed

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Delete Feed

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Create Feed

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds -d

    {
        "access": 1,
        "alias": "alias",
        "catid": 5,
        "description": "",
        "images": {
            "float_first": "",
            "float_second": "",
            "image_first": "",
            "image_first_alt": "",
            "image_first_caption": "",
            "image_second": "",
            "image_second_alt": "",
            "image_second_caption": ""
        },
        "language": "*",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadata": {
            "hits": "",
            "rights": "",
            "robots": "",
            "tags": {
                "tags": "",
                "typeAlias": null
            }
        },
        "metadesc": "",
        "metakey": "",
        "name": "Name",
        "ordering": 1,
        "params": {
            "feed_character_count": "",
            "feed_display_order": "",
            "newsfeed_layout": "",
            "show_feed_description": "",
            "show_feed_image": "",
            "show_item_description": ""
        },
        "published": 1
    }

#### Update Feed

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds/{feed_id} -d

    {
        "access": 1,
        "alias": "test2",
        "catid": 5,
        "description": "",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadesc": "",
        "metakey": "",
        "name": "Test"
    }

### Categories

1.  Route Newsfeeds Categories is: "v1/newsfeeds/categories"
2.  Working with it is similar to [Banners Categories](#Categories).

## Privacy

### Request

#### Get List of Requests

curl -X GET /api/index.php/v1/privacy/request

#### Get Single Request

curl -X GET /api/index.php/v1/privacy/request/{request_id}

#### Get Single Request Export Data

curl -X GET /api/index.php/v1/privacy/request/export/{request_id}

#### Create Request

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d

    {
        "email":"somenewemail@com.ua",
        "request_type":"export"
    }

### Consent

#### Get List of Consents

curl -X GET /api/index.php/v1/privacy/consent

#### Get Single Consent

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Delete Consent

curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}

## Redirect

### Redirect

#### Get List of Redirects

curl -X GET /api/index.php/v1/redirect

#### Get Single Redirect

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Delete Redirect

curl -X DELETE /api/index.php/v1/redirect/{redirect_id}

#### Create Redirect

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/redirect -d

    {
        "comment": "",
        "header": 301,
        "hits": 0,
        "new_url": "/content/art/99",
        "old_url": "/content/art/12",
        "published": 1,
        "referer": ""
    }

#### Update Redirect

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

    {
        "new_url": "/content/art/4",
        "old_url": "/content/art/132"
    }

## Tags

### Tags

#### Get List of Tags

curl -X GET /api/index.php/v1/tags

#### Get Single Tag

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Delete Tag

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Create Tag

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d

    {
        "access": 1,
        "access_title": "Public",
        "alias": "test",
        "description": "",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "path": "test",
        "published": 1,
        "title": "test"
    }

#### Update Tag

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d

    {
        "alias": "test",
        "title": "new title"
    }

## Templates

### Templates Styles

#### Get List of Templates Styles

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Get Single Template Style

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Delete Template Style

curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Create Template Style

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

    {
        "home": "0",
        "params": {
            "fluidContainer": "0",
            "logoFile": "",
            "sidebarLeftWidth": "3",
            "sidebarRightWidth": "3"
        },
        "template": "cassiopeia",
        "title": "cassiopeia - Some Text"
    }

#### Update Template Style

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d

    {
        "template": "cassiopeia",
        "title": "new cassiopeia - Default"
    }

## Users

### Users

#### Get List of Users

curl -X GET /api/index.php/v1/users

#### Get Single User

curl -X GET /api/index.php/v1/users/{user_id}

#### Delete User

curl -X DELETE /api/index.php/v1/users/{user_id}

#### Create User

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users
-d

    {
        "block": "0",
        "email": "test@mail.com",
        "groups": [
            "2"
        ],
        "id": "0",
        "lastResetTime": "",
        "lastvisitDate": "",
        "name": "nnn",
        "params": {
            "admin_language": "",
            "admin_style": "",
            "editor": "",
            "helpsite": "",
            "language": "",
            "timezone": ""
        },
        "password": "qwertyqwerty123",
        "password2": "qwertyqwerty123",
        "registerDate": "",
        "requireReset": "0",
        "resetCount": "0",
        "sendEmail": "0",
        "username": "ad"
    }

#### Update User

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d

    {
        "email": "new@mail.com",
        "groups": [
            "2"
        ],
        "name": "name",
        "username": "username"
    }

### Fields Users

1.  Route Fields Users is: "v1/fields/users"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").

### Groups Fields Users

1.  Route Groups Fields Users is: "v1/fields/groups/users"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Groups_Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").
