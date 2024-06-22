<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Form Validation -->

## Introduction

Joomla has a client-side validation script that can be used and extended by any component. This article is about how it works and how to use it. There are four standard validators for username, password, numeric and email field types. There is also a more general purpose pattern validator and a required field validator.

Note that modern browsers also provide form validation by default. Browser validation can be turned off by including novalidate in the form tag.

## How to invoke validation

At the top of the edit.php file containing the form generation code include the call to load the validator javascript file:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
	->useScript('form.validate');
```

in the form tag make sure to include class="form-validate". This example is from the user edit form:

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
	method="post" name="adminForm" id="user-form"
	enctype="multipart/form-data"
	aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
	class="form-validate">
```

In the form xml file include the statements that invoke individual field validation. This example is the user email field:

```xml
		<field
			name="email"
			type="email"
			label="JGLOBAL_EMAIL"
			required="true"
			size="30"
			validate="email"
			validDomains="com_users.domains"
		/>
```

## The Validation Expressions

This section is intended to give a little understanding of the validation expressions to help explain usage. All are included in the validator.js code.

### Username

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Here the expression is looking for the occurrence of any of the alternative characters within the character class [] and returning false (not valid) if any is found.

### Password

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Here the expression is looking for a leading character that is not white space followed by 2 to 98 characters that are either not white space or a space and ending with a non-white space character. Joomla has a more sophisticated password checker that ensures a mix of character types and minimum length.

### Email

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Here the regular expression accepts any string that starts with one or more of the characters in the [] character class, followed by @, followed by any one or more characters in the second [] character class, followed by zero or more full stops followed by one or more characters in the third character class group, and ending with one of the groups after the @.

Although technically correct, this validator allows some quite silly email addresses such as !@me - you might want to create a more restrictive pattern.

### Numeric

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Here the value must start with a digit or minus sign zero or one times, be followed by a digit or a comma zero or more times, followed by a full stop zero or one times, ending with a digit zero or more times. So this will match 3.142 or -10 or 100,000.0 and so on. The actual number needs to match whatever the field type is in the database. The expression will not match numbers with exponents, so 10e2 will be rejected.

### Pattern

Suppose you want an input field to be a positive integer. You could use your own pattern placed in the form xml file:

```xml
		<field
			name="number"
			type="text"
			label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
			description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
			class="w-auto"
			required="true"
			pattern="\d+"
		/>
```

Here the pattern allows one or more digits. So -1 would be invalid, as would 3.142.
Required

Make sure to include required="true" in the field specification or the form label will omit the commonly used * marker used to indicate required fields. Including required in the class is not enough (is this a bug?).