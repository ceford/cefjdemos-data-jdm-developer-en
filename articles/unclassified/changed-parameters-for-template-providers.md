<!-- Filename: J4.x:Changed_parameters_for_template_providers / Display title: Changed parameters for template providers -->

Starting with Joomla 4.0 we changed / removed and added some options to
the core extensions that might affect template providers.

## Errors reported

Some options have been removed or changed in comparision to 3.x

## Versions affected

General Information

This pertains only to Joomla! version(s): **4.0.0+**

## What is the cause

This is an expected behavior as of Joomla 4.0

The following options has been changed comparing the current 3.10-dev
vs. 4.0-dev (as of 09.09.2020):

```
    com_config/config/default:
      Fields added in J4:
        request/request/controller

    com_config/templates/default:
      Fields added in J4:
        request/request/controller

    com_contact/categories/default:
      Fields removed in J4:
        params/contact/presentation_style

    com_contact/category/default:
      Fields added in J4:
        params/advanced/contacts_display_num
      Fields removed in J4:
        params/contact/presentation_style

    com_contact/contact/default:
      Fields removed in J4:
        params/params/presentation_style

    com_contact/featured/default:
      Fields removed in J4:
        params/contact/presentation_style

    com_content/article/default:
      Fields removed in J4:
        params/basic/show_icons
        params/basic/show_print_icon
        params/basic/show_email_icon

    com_content/categories/default:
      Fields removed in J4:
        params/blog/num_columns
        params/blog/multi_column_order
        params/article/show_icons
        params/article/show_print_icon
        params/article/show_email_icon

    com_content/category/blog:
      Fields added in J4:
        params/advanced/blog_class_leading
        params/advanced/blog_class
      Fields removed in J4:
        params/advanced/bloglayout
        params/advanced/num_columns
        params/advanced/multi_column_order
        params/article/show_icons
        params/article/show_print_icon
        params/article/show_email_icon

    com_content/category/default:
      Fields removed in J4:
        params/article/show_icons
        params/article/show_print_icon
        params/article/show_email_icon

    com_content/featured/default:
      Fields added in J4:
        params/advanced/blog_class_leading
        params/advanced/blog_class
      Fields removed in J4:
        params/advanced/num_columns
        params/advanced/multi_column_order
        params/article/show_icons
        params/article/show_print_icon
        params/article/show_email_icon

    com_finder/search/default:
      Fields added in J4:
        params/basic/show_taxonomy
        params/basic/show_date
```

## How to fix

When you have a template that supports both 3.x and 4.x please make sure
that you take care of the changed parameter set.
