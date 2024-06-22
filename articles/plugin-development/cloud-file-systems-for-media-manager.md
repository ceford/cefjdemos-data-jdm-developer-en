<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Cloud File Systems for Media Manager -->

<span id="main-portal-heading">GSoC 2017  
Cloud File Systems for Media Manager  
Documentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2017 "GSOC 2017")
Joomla!  4.x

## Introduction

**Joomla! 4.x** is shipped with cloud file systems for the **Media
Manager** by default. With the previous API, creating custom file
systems was a difficult task. Thanks to the new API, it's now easy to
create a custom file system. If you want to use a Cloud Service with the
new Media Manager, it is advised to use **OAuth2.0**.

This document will guide you through important steps to create your own
**File System Plugin** to extend the **Media Manager**. Before
proceeding, please make sure you have the basic knowledge on how to
develop a plugin for Joomla. [This
tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla")
should help.

## Create your Filesystem plugin

### Configuration

First of all, we need to create a **filesystem** plugin to extend the
Media Manager. This plugin should contain some important attributes so
that it can work with the Media Manager.

Make sure your plugin contains `group="filesystem"`. So in your
`[plugin-name].xml`, you should have:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
	<name>plg_filesystem_myplugin</name>
	<author>Joomla! Project</author>
	<creationDate>April 2017</creationDate>
	<copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>__DEPLOY_VERSION__</version>
	<description>Description</description>
	<files>
		<filename plugin="myplugin">myplugin.php</filename>
		<folder>SomeFolder</folder>
	</files>
	
	<config>
		<fields name="params">
			<fieldset name="basic">
				<field
					name="display_name"
					type="text"
					label="YOUR_LABEL"
					description="YOUR_DESCRIPTION"
					default="DEFAULT_VALUE"
				/>
			</fieldset>
		</fields>
	</config>
</extension>
```

The **display_name** parameter helps the Media Manager to display the
name of your **File System** as a root node in the File Browser. Any
adapter belonging to your File System will be displayed as children
under the root.

Include any required parameters you may need such as `App Secret` with
suitable Form Fields.

### Plugin Events

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Any** Filesystem plugin should contain an event called
`onFileSystemGetAdapters()` for functionality. This event should return
an **array** of
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` when it
is called. The event is raised when you open the Media Manager. Each
`AdapterInterface` will be mounted under the root node of your file
system.

#### onFileSystemOAuthCallback()

This event is fired when you used Media Manager's
**OAuthCallbackHandler** for OAuth2.0 Authorization and Authentication
process described below in the document. The event is only fired on the
requested plugin. So there's no need to check for `$context` in a
typical scenario.

An example of use of the event looks like:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here
        
        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];
        
        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** contains the input forwarded to the Media Manager
OAuthCallback URI. `$event->getInput()` returns an Input Object.

`$result` defines how Joomla behaves after a redirect to the callback.
There are several possible solutions available:

- `action`
  - close: Closes a window which is opened by a JavaScript **using
    window.open()**
  - redirect: Redirects to the page specified by the **redirect_uri**
  - control-panel: Redirect to the control panel
  - media-manager: Redirect to Media Manager
- `redirect_uri`
  - Used with **redirect** action (required)
- `message`
  - Message needs to be displayed after an action
- `message_type`
  - warning
  - notice
  - error
  - message(or set empty)

After setting everything, set the `$result` argument of the `$event` to
pass it back to the caller.

An example to display a message to user would be:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

This will redirect to the Media Manager and raise a notice with the text
in **message**.

This method can be typically used to obtain authorization codes for the
**OAuth2.0** process. In case of any **error**, Joomla! will fallback to
the control panel adn will display an error message.

## Authentication and Authorization

Joomla! 4.x advices you to use OAuth2.0 for this process. It is a common
scenario that OAuth2.0 requires a **redirect url** to pass
authentication data to the Application. This is typically done by a
callback handler. For your plugin, no need to reinvent the wheel, you
can use the **Callback Handler** of **Media Manager** for fulfilling the
task.

All you have to do is to set the **Redirect URI** to following in your
OAuth2.0 Provide and Implement the
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
in your plugin.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

In this example:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Now when you do a redirect from your provider once it reaches the URL
provided, the **Controller** for Media Manager will ensure that your
plugin implements
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
before passing any data to it. If not, you will be redirected to the
Control Panel with an error message.

If you have implemented all the inputs that are passed with, the Cloud
Provider will be included in the `$event`. You can access them using
`$event->getInput()` and treat it as an usual `input` to Joomla.

After you received the `input`, you can use it to obtain the details of
your cloud service such as **Access Token**, **Refresh Token** etc.

If you want to distinguish multiple accounts, you can use `Session` for
that.

**Note**: It is advised to check against **CSRF token** before you
proceed your request. Most cloud providers support `state` parameter
that can be used to fulfill the task.

### Common Pitfalls

It is required to `urlencode()` your redirect uri when you're sending it
to the cloud provider. Please use it to avoid misbehaviours of your
cloud.

## File Serve from your Adapter

To serve your media files from the Media Manager to **Joomla! Site**
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` helps
you. This Interface contains a special method called `getUrl($path)`.

`$path : Path is relative to your root`

The intention of the method is to provide a **Public Absolute URL** for
the file you requested to insert in the site. For example assume your
file path is `/path/to/me.png` in the cloud server. Now a public URL
might be something like `mycloud.com/share/u/myusername/path/to/me.png`.
How it is being served, is completely up to you. When an user wants to
insert a media generated URL, this method will be used.

More details about `Adapter Interface` can be found in
`administrator/componenents/com_media/Adapter/AdapterInterface.php`
