<!-- Filename: J4.x:Writing_A_CLI_Application / Display title: Writing A CLI Application -->

## Introduction

Joomla 4 comes with an updated way of writing CLI Commands. We'll try
and cover the most important changes here and teach you how to write
your own CLI commands in Joomla 4.

## Brief Overview of changes in Joomla 4

When you built a CLI application in Joomla 4 you'd create a full
Application instance by extending JApplicationCli. In Joomla 4 instead
of building up a full new application instance we've built a new Joomla
Application that uses the
<a href="https://symfony.com/doc/current/components/console.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Symfony Console component</a> to make
the process of integrating your components much easier than it was
previously. The Joomla 3 way of building an application through
JApplication will work in Joomla 4, however is deprecated and will be
removed in Joomla 5.

The new <a href="https://github.com/joomla-framework/console"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Console Package</a> (a new package in
version 2 of the
<a href="https://framework.joomla.org/" class="external text"
target="_blank" rel="noreferrer noopener">Joomla Framework</a>) contains
the base code and it's extended in the CMS to add support for things
like using JInput.

The advantage of the new way is that we have a single application that
means using `JApplication::isClient('cli')` will work. The introduction
of *\\Joomla\\CMS\\Application\\CMSApplicationInterface* means that all
applications within Joomla should have a consistent set of methods
available for extensions to use.

## Creating a Command

The most rudimentary command just prints a title and some text into the
terminal.

```php
       /**
         * The default command name
         *
         * @var    string
         * @since  4.0.0
         */
        protected static $defaultName = 'commandgroup:commandname';

        /**
         * Internal function to execute the command.
         *
         * @param   InputInterface   $input   The input to inject into the command.
         * @param   OutputInterface  $output  The output to inject into the command.
         *
         * @return  integer  The command exit code
         *
         * @since   4.0.0
         */
        protected function doExecute(InputInterface $input, OutputInterface $output): int
        {
            $symfonyStyle = new SymfonyStyle($input, $output);

            $symfonyStyle->title('Hello World Command Title');

            // You might want to do some stuff here in Joomla

            $symfonyStyle->success('Hello World!');

            return 0;
        }

        /**
         * Configure the command.
         *
         * @return  void
         *
         * @since   4.0.0
         */
        protected function configure(): void
        {
            $this->setDescription('This command prints hello world to whoever calls it');
            $this->setHelp(
                <<%command.name% command prints hello world to whoever calls it
    php %command.full_name%
    EOF
            );
        }
```

Let's cover the 3 parts of this code:

- *configure* function: Configures information about the command setting
  up information in the class which is shown when a user lists help
  information for the command *doExecute* function: This method does
  the work of the command - you'd normally go and execute a Joomla
  Component here (i.e. Dispatch a components dispatcher) *defaultName*
  property: This is the command name that is run when executed (remember
  this we'll come back to it in a second!)If you need more help there's
  an excellent Video produced as part of our [GSOC
  2018](https://docs.joomla.org/J4.x:CLI_Update "J4.x:CLI Update")
  program to help you out!

## Adding the command to the Application by Plugin

Inside your plugin you're going to register to the event
*\Joomla\Application\ApplicationEvents::BEFORE_EXECUTE* using the method
*getSubscribedEvents*. The following Example required a command
*RunHelloCommand* and registers it.

```php
    class PlgsystemMyConsole extends CMSPlugin implements SubscriberInterface
    {

        public static function getSubscribedEvents(): array
        {
            return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
            ];
        }

        public function registerCommands(): void
        {
            $serviceId = 'hello.world';

            Factory::getContainer()->share(
                $serviceId,
                function (\Psr\Container\ContainerInterface $container) {
                    // do stuff to create command class and return it
                    return new RunHelloCommand();
                },
                true
            );

            Factory::getContainer()->get(\Joomla\CMS\Console\Loader\WritableLoaderInterface::class)->add('hello:world', $serviceId);
        }

    }
```

This method *registerCommands* adds your command into the Dependency
Injection Container then registers that command against the name of the
command for when it's called by the Joomla CLI Application (note the
first parameter of `WritableLoaderInterface::add()` should be the same
as the *defaultName* property inside your Command Class.

## Calling the Command

To call the command you just created you need to run it through the
Joomla CLI Command:

```php
    php cli/joomla.php my:command
```
