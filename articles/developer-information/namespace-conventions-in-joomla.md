<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Namespace Conventions In Joomla -->

Joomla!  4.0

### Base Namespaces

The recommended naming conventions in Joomla 4 are the following:

### Components

#### Core Components

- Joomla\Component\\*ComponentName*\\*ClientName*
  - Joomla\Component\Content\Site
  - Joomla\Component\Content\Administrator

#### 3rd Party Components

- *CompanyName*\Component\\*ComponentName*\\*ClientName*
  - Acme\Component\Backup\Site
  - Acme\Component\Backup\Administrator

#### Example locations

*Joomla\Component\Content\Site\Controller*  
components/com_content/Controller/ArticleController.php

*Joomla\Component\Content\Administrator\Controller*  
administrator/components/com_content/Controller/ArticlesController.php

### Modules

#### Core Modules

- Joomla\Module\\*ModuleName*\\*ClientName*
  - Joomla\Module\UsersLatest\Site
  - Joomla\Module\Quickicon\Administrator

#### 3rd Party Modules

- *CompanyName*\Module\\*ModuleName*\\*ClientName*
  - Acme\Module\SubscriptionStatus\Site
  - Acme\Module\LastBackupStatus\Administrator

#### Example locations

*Joomla\Module\Quickicon\Administrator\Dispatcher*  
administrator/modules/mod_quickicon/Dispatcher/Dispatcher.php

*Joomla\Module\UsersLatest\Site\Helper*  
modules/mod_users_latest/Helper/UsersLatestHelper.php

### Plugins

#### Core Plugins

- Joomla\Plugin\\*PluginGroup*\\*PluginName*
  - Joomla\Plugin\System\Debug
  - Joomla\Plugin\Quickicon\Joomlaupdate

#### 3rd Party Plugins

- *CompanyName*\Plugin\\*PluginGroup*\\*PluginName*\>
  - Acme\Plugin\System\BackupOnDemand

#### Example locations

*Joomla\Plugin\System\Debug*  
plugins/system/debug/DataFormatter.php

*Joomla\Plugin\Quickicon\Joomlaupdate\Extension*  
plugins/quickicon/joomlaupdate/Extension/Joomlaupdate.php

### Framework

#### Joomla Framework Library

- Joomla
  - Joomla\Session
  - Joomla\Image

#### Joomla CMS Library

- Joomla\CMS
  - Joomla\CMS\Mail
  - Joomla\CMS\MVC

#### Example locations

*Joomla\Session*  
libraries/vendor/joomla/session/src/Session.php

*Joomla\CMS\Mail*  
libraries/src/Mail/Mail.php
