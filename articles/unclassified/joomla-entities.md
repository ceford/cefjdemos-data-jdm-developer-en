<!-- Filename: J4.x:Joomla_Entities / Display title: Joomla Entities -->

<span id="main-portal-heading">GSoC 2018  
Joomla Entities  
Documentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Introduction

The scope of this year project is to build and integrate a new Model
system into Joomla 4, that will solve some of the current issues from
the Webservices project, as well as improve the limits of the current
system. The integration will be fully implemented by the release of
Joomla 4.0 in the com_content component. The purpose for this
documentation is to outline how to use the underlaying Entities project,
which can be then used by developers to write new extensions and convert
existing ones.

Entities! The entities project is the base project. Without it we would
not be able to accomplish the tasks in the Webservices project. Starting
as a new project, I have managed to get a basic implementation going of
an Active Record pattern based layer that shall be used as a dependency
in Joomla CMS, to implement each table as a DAO.

The need for this project comes from the limitations of the current
Model architecture which have been reached during the last year GSoC
Webservices <a
href="https://joomla-projects.github.io/gsoc18_webservices/?specification/introduction.md"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">project</a>.

Because by reaching these limitations, it was clear that the Model layer
had to be redone. Taking it a step further, with the desire to make
development in Joomla easier, the Entities
<a href="https://github.com/joomla-projects/entities"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">project</a> was born. The main
highlights of this approach is that we can now have relations in our
models, feature that has been long missed in Joomla models. Up to now,
the One to One and One To Many relations have been implemented.

The project fits perfectly in the Joomla ecosystem, as it is using the
joomla-framework/database DatabaseDriver for the connection to various
database systems. We have decided that the next step would be actually
integrating the Entities project in Joomla CMS, in the lib_api branch of
the last year webservices project. Taking a step back from implementing
features in a separate project, and actually connecting the dots with
the CMS implementation will give us important insights about the
features that will be needed in the future.

## Installation

The entities project can be easily integrated into any php project by
adding the following dependency to your composer file:

```json
    "config": {
        "preferred-install": {
            "joomla/event": "dist"
        },
        "require": {
            "joomla/entities": "dev-master"
        }
    }
```

## Usage

### Defining Entities

- Table - The table name should have '#\_\_' as a prefix. The Entity
  also sets a default table name in case none is specified by the
  developer. The default value is the pluralised and underscore
  separated Class name.

```php
       /**
         * The table associated with the model.
         *
         * @var string
         */
        protected $table = '#__content';
```

- Timestamps - specify is the current entity uses timestamps or not. If
  it does, special column aliases need to be specified for the
  'createdAt' and 'updatedAt' columns. These two columns are handled
  automatically by the project when creating or updating the entity.

```php
       /**
         * Indicates if the model should be timestamped.
         *
         * @var boolean
         */
        public $timestamps = false;
```

- Column Aliases - having the same functionality as in the current
  Joomla implementation, they provide a way to be able to refer at
  different column names across different components with one name in
  case they fulfil the same functionality.

```php
       /**
         * Array with alias for "special" columns such as ordering, hits etc etc
         *
         * @var    array
         */
        protected $columnAlias = [
            'createdAt' => 'created',
            'updatedAt' => 'modified',
            'published' => 'state'
        ];
```

- Casts - Used to cast database raw strings into usable types in PHP.
  Two most common use cases are 'int' and 'array'. The latter is
  specifically useful as it transforms json columns into key indexed
  arrays.

```php
       /**
         * The attributes that should be cast to native types.
         *
         * @var array
         */
        protected $casts = [
            'params' => 'array'
        ];
```

- Dates - used to cast dates to Carbon instances

```php
       /**
         * The attributes that should be mutated to dates. Already aliased!
         *
         * @var array
         */
        protected $dates = [
            'registerDate',
            'lastvisitDate',
            'lastResetTime'
        ];
```

- Hidden - used to hide certain columns from serialisation. E.g.
  passwords.

```php
       /**
         * The attributes that should be hidden for serialization.
         *
         * @var array
         */
        protected $hidden = [
            'password'
        ];
```

- With - eager load relations into the entity. If not specified, the
  relations are lazy loaded. The primary key and foreign key are
  mandatory if the relation is loaded with constraints(selecting only
  some of the columns for efficiency purposes)

```php
       /**
         * The relations to eager load on every query.
         *
         * @var array
         * @todo add to docs: 
         */
        protected $with = array(
            'sentMessages:message_id,subject,user_id_from'
        );
```

### Defining Entities Relations

- HasOne - One to One relation.

Signature:

```php
       /**
         * Define a one-to-one relation.
         *
         * @param   string  $related    related Model
         * @param   string  $foreignKey foreign key name in current mode
         * @param   string  $localKey   local primary key name
         * @return \Joomla\Entity\Relations\HasOne
         */
        public function hasOne($related, $foreignKey = null, $localKey = null)
```

Example:

```php
       /**
         * Get the profile for the current user.
         * @return Relation
         */
        public function profile()
        {
            return $this->hasOne('Joomla\Entity\Tests\Models\UserProfile');
        }
```

- HasMany - One to Many relation.

Signature:

```php
       /**
         * Define a one-to-many relation.
         *
         * @param   string  $related    related Model
         * @param   string  $foreignKey foreign key name in current mode
         * @param   string  $localKey   local primary key name
         * @return \Joomla\Entity\Relations\HasMany
         */
        public function hasMany($related, $foreignKey = null, $localKey = null)
```

Example:

```php
       /**
         * Get the sent messages for the current user.
         * @return Relation
         */
        public function sentMessages()
        {
            return $this->hasMany('Joomla\Entity\Tests\Models\Message', 'user_id_from');
        }
```

- BelongsTo - Reverse relation for One to One or One to Many.

Signature:

```php
       /**
         * Define an inverse one-to-one or many relation.
         *
         * @param   string  $related    related Model
         * @param   string  $relation   relation name, must be the same as the caller function
         * @param   string  $foreignKey foreign key name in current mode
         * @param   string  $ownerKey   the associated key on the parent model.
         * @return \Joomla\Entity\Relations\BelongsTo
         */
        public function belongsTo($related, $relation, $foreignKey = null, $ownerKey = null)
```

Example:

```php
       /**
         * Get the author for the current article.
         *
         * @return Relation
         */
        public function author()
        {
            return $this->belongsTo('Joomla\CMS\Entity\User', 'author', 'created_by');
        }
```

### Queries Using The Entity

The following default functionalities are available for the entities:

- Find - finds an entity or a Collection of entities by their primary
  keys. In addition, the columns that are desired to be loaded can be
  specified. By default, we load all the columns.

Signature:

```php
       /**
         * Find a model by its primary key.
         *
         * @param   mixed  $id      primary key
         * @param   array  $columns columns to be selected in query
         *
         * @return Model|boolean
         */
        public function find($id, $columns = ['*'])
```

- Insert - This is the way to insert a new row in a table. First we
  create a new entity using the desired attribute, next we persist it to
  the database.

Example:

```php
           $attributes = [
                'email' => "test@test.com",
                'params' => ['test' => 'val']
                ];

            $user = new User(self::$driver, $attributes);

            $user->persist();
```

- Update - This is the way to update an existing entity. Entities
  attributes can be accessed as normal class properties using magic
  methods.

Example:

```php
           $model = new User(self::$driver);

            $user = $model->find(100);
            $user->resetCount = 10;

            $user->update();
```

- Delete - This is a way to delete a row from the table using it's
  primary key.

Example:

```php
           $model = new User(self::$driver);

            $user = $model->find(100);
            $user->resetCount = 10;

            $user->update();
```

- Get - The get method returns all the models that are in the table,
  respectively, all the filters that have been applied to the entity so
  far. Please check the list of filter methods at the end of this
  section for more details.

Signature:

```php
       /**
         * Finds all the Models with eager relations loaded
         *
         * @param   array  $columns columns to be selected in query
         * @return Collection
         */
        public function get($columns = ['*'])
```

- Count - The count method counts the all the rows from the table,
  respectively all the rows that satisfy filters that have been applied
  to the entity so far.

Example:

```php
           $model = new User(self::$driver);
            $count = $model->count();
```

#### Filtering

- select - selects only the specified columns in the next query.
     /* @method select()     select(array $columns) */

- where -\> adds a where clause in the next query that will be executed.
     /* @method where()      where($conditions, string $glue = 'AND') */

- orders -\> adds an ORDER BY clause to the next query.
     /* @method order()      order($columns) */

- filter - applies a custom filter on a relation. This is the Entities
  way of making joins.
```php
     /* @method filter()     filter(string $relation, Closure $callback) */

        // The following query only counts users which sent a message with subject "sentMessages".
        $model = new User(self::$driver);
        $model->filter('sentMessages',
        function ($query)
        {
            $query->where("subject = 'message1'");
        }
        );
        $count = $model->count();
```
