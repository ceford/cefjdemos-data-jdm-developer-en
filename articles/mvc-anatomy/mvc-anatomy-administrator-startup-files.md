<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Anatomy: Administrator Startup Files -->

## Administrator Files Overview

There are more files in the administrator part of the component, partly because there are two tables, each with a list and edit view, and partly because some of the files control aspects of component behaviour in both site and administrator interfaces.

Creation of code for a list view has been covered in the part of the tutorial dealing with the site files so will not be repeated here. This section will deal with the files common to most components. A subsequent tutorial will cover one of the edit views required to update or add new records.

## services/provider.php

This file used when the Joomla application calls the ComponentHelper to render the component. It is the very first component code executed for both Site and Administrator. Its role is to register the component:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extension/CountrybaseComponent.php

The register function of this file is called from the \$component = new CountrybaseComponent(...); statement in services/provider.php. Its purpose is to boot the component in both Site and Administrator interfaces.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

This file sets the access controls used in this component. The items listed here appear in the component Options Permissions tab.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

This file is used to control whether any options appear in the component *Options* form and which tab they appear under. For com_countrybase there are no options other than Joomla standard permissions options. It would be possible to add options here on, for example, whether to show or hide a particular column in the output display. That would go in a separate fieldset named Options.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```
