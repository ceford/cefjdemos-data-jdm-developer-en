<!-- Filename: J4.x:Creating_an_Authentication_Plugin_for_Joomla / Display title: Creating an Authentication Plugin for Joomla -->

The authentication plugin system for Joomla! offers a great deal of
flexibility and power to the system. Using the system, it is possible to
authenticate users from any source - the Joomla! internal database, the
Open ID system, an LDAP directory, or any authentication system that can
be accessed using PHP.

This tutorial will present a really basic example of an authentication
plugin that demonstrates how to create custom authentication plugins for
the Joomla! CMS.

## The PlgAuthenticationMyauth Class

Joomla! plugins are typically created by creating a child class of the
\Joomla\CMS\Plugin\CMSPlugin class. The \Joomla\CMS\Plugin\CMSPlugin
class provides all the infrastructure and basic functionality that is
required. All that is necessary is to provide the necessary methods to
handle the desired event.

To create an authentication plugin, the name of the child class must
begin with `PlgAuthentication`, and must end with the name of the plugin
that is being created. In our case, the plugin is called Myauth, so the
class will be called `PlgAuthenticationMyauth`.

The class will have just two methods - the first `getSubscribedEvents()`
which tells Joomla what events our plugin is listening for (just the
[*onUserAuthenticate*](https://docs.joomla.org/Plugin/Events/Authentication#onUserAuthenticate "Plugin/Events/Authentication")
event for an Authentication Plugin) and the second `authenticate()`
method which actually does the authentication. Both methods are actually
very simple, as will be demonstrated.

## The onAuthenticate Event

The `onAuthenticate` event is the Joomla Event that will be called when
the system is trying to use your plugin to authenticate the user. This
contains three pieces of data: the credentials, some extra options, and
a reference to an object of type
\Joomla\CMS\Authentication\AuthenticationResponse. Our `authenticate()`
method needs to determine if the username and password are a valid
combination for authentication and return the result in the
AuthenticationResponse object.

For our example, the authentication check that we are going to do is
very simple. We will simply make sure that the specified username exists
in the users table, and if it does, we will check to see if the username
is the reverse of the password. So our authentication check will look
like:

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName('id'))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

    $this->db->setQuery($query);
    $result = $this->db->loadResult();

    /**
     * To authenticate, the username must exist in the database, and the password should be equal
     * to the reverse of the username (so user joeblow would have password wolbeoj)
     */
    if($result && ($credentials['username'] == strrev( $credentials['password'] )))

Although this is very basic in our example, this code can be replaced
with any code that is necessary to perform the authentication checking
that is necessary for your plugin. The flexibility is only limited by
what PHP can do.

Now that we have determined whether or not authentication was
successful, we can now create our response:

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName('id'))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

    $this->db->setQuery($query);
    $result = $this->db->loadResult();

    if (!$result) {
        $response->status = \Joomla\CMS\Authentication\Authentication::STATUS_FAILURE;
        $response->error_message = 'User does not exist';
    }
    /**
     * To authenticate, the username must exist in the database, and the password should be equal
     * to the reverse of the username (so user joeblow would have password wolbeoj)
     */
    if($result && ($credentials['username'] == strrev( $credentials['password'] )))
    {
        $email = \Joomla\CMS\User\User::getInstance($result); // Bring this in line with the rest of the system
        $response->email = $email->email;
        $response->status = \Joomla\CMS\Authentication\Authentication::STATUS_SUCCESS;
    }
    else
    {
        $response->status = \Joomla\CMS\Authentication\Authentication::STATUS_FAILURE;
        $response->error_message = 'Invalid username and password';
    }

For failed responses, we set two properties of the response object: the
status property, and the error_message property. Currently there are six
recognized response status value - `STATUS_SUCCESS`, `STATUS_FAILURE`,
`STATUS_CANCEL`, `STATUS_EXPIRED`, `STATUS_DENIED` and `STATUS_UNKNOWN`.
For more information on these status values, consult the
libraries/src/Authentication/authentication.php file.

The error_message property is set in case the authentication is not
successful. In our plugin, we set two possible values to this property:
"User does not exist", which indicates that our query did not return any
results, and "Invalid username and password", which indicates that the
password was not the reverse of the username. It should be noted that
these values are not returned to the user. For security reasons, the
only thing the user will see is a successful login, or a message that
says, "Username and password do not match." The Joomla! system can be
configured so that these error messages can be stored in a log file for
debugging purposes.

If authentication is successful, we can optionally add information from
our authentication source to the response. In this case, we are
retrieving the user information from the Joomla! database and storing
the email address in the response object. For more information on what
data can be stored in the response object, please consult
<img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" />. This data can then be used by user
plugins in the event it is desired to automatically create users or
perform other login tasks.

## The Complete myauth.php File

Now that we have completed the two methods that are necessary for our
class, we put our class into a PHP file that has the same name as our
plugin. Since our plugin is called Myauth, we call our file myauth.php.
Here is the complete listing for this file:

     'authenticate',
            ];
        }

        /**
         * This method should handle any authentication and report back to the subject
         * This example uses simple authentication - it checks if the password is the reverse
         * of the username (and the user exists in the database).
         *
         * @access    public
         * @param     array     $credentials    Array holding the user credentials ('username' and 'password')
         * @param     array     $options        Array of extra options
         * @param     object    $response       Authentication response object
         *
         * @return    boolean
         *
         * @since 1.0
         */
        public function authenticate( $credentials, $options, &$response )
        {
            /*
             * Here you would do whatever you need for an authentication routine with the credentials
             *
             * In this example the mixed variable $return would be set to false
             * if the authentication routine fails or an integer userid of the authenticated
             * user if the routine passes
             */
            $query = $this->db->getQuery(true)
                ->select($this->db->quoteName('id'))
                ->from($this->db->quoteName('#__users'))
                ->where($this->db->quoteName('username') . ' = :username')
                ->bind(':username', $credentials['username']);

            $this->db->setQuery($query);
            $result = $this->db->loadResult();

            if (!$result)
            {
                $response->status = STATUS_FAILURE;
                $response->error_message = 'User does not exist';
            }

            /**
             * To authenticate, the username must exist in the database, and the password should be equal
             * to the reverse of the username (so user joeblow would have password wolbeoj)
             */
            if($result && ($credentials['username'] == strrev( $credentials['password'] )))
            {
                $email = User::getInstance($result); // Bring this in line with the rest of the system
                $response->email = $email->email;
                $response->status = Authentication::STATUS_SUCCESS;
            }
            else
            {
                $response->status = Authentication::STATUS_FAILURE;
                $response->error_message = 'Invalid username and password';
            }
        }
    }

## The XML Install Manifest

Now that we have created our CMSPlugin class, all we have to do is
create our XML install file that will tell the Joomla! installer how to
install our plugin. This file is simple:


        Authentication - Myauth
        Joomla! Documentation Project
        May 30, 2007
        (C) 2005 - 2020 Open Source Matters. All rights reserved.
        GNU General Public License version 2 or later; see LICENSE.txt
        admin@joomla.org
        www.joomla.org
        1.0
        An sample authentication plugin
        
            myauth.php
        
        

You will notice that this file looks very similar to any other Joomla!
XML install manifest file. There are a few important things to notice.

The first thing to notice is the group attribute on the root element.
For authentication plugins, the group attribute must have the value
'authentication'. This tells the Joomla! system to treat your plugin as
an authentication plugin.

We entered the name 'Authentication - Myauth' in the name field. Your
plugin doesn't HAVE to follow this convention, but it looks better
because then it will match the standard authentication plugins that are
listed in the plugin manager.

Finally, notice that filename attribute that contains our plugin file
has an attribute called plugin. The value of this should be the name of
our plugin. In this case, it is myauth.

There are many more properties available in this file. Some can also be
translated. For the full information please read the [Manifest
files](https://docs.joomla.org/Manifest_files "Manifest files")
tutorial.

## Wrapping it All Up and Using It

Now that we have created our two files, all we have to do is package
them up into an archive file that can be read by the Joomla! installer
system.

Once we package and install our plugin, it is ready to be used. The
plugin is published using the Plugin Manager. All of the authentication
plugins will be grouped together. Plugins are enabled by 'publishing
them'. You can publish as many authentication plugins as you want. In
order for successful authentication to occur, only one of the plugins
needs to return a
`\Joomla\CMS\Authentication\Authentication::STATUS_SUCCESS` result.
Remember to always keep at least one authentication plugin enabled so
you don't get locked out!

## Conclusion

We have now created a simple authentication plugin. We have demonstrated
the basic process of doing an authentication check and return the
results to the Joomla! system.

You can also easily test this plugin by packaging it yourself.
