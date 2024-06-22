<!-- Filename: J4.x:ClientSideEvents / Display title: ClientSideEvents -->

## Concept

When writing a complex site with many scripts, components, plugins it is
become important that every script is able to talk each other. For this
Joomla! introduces Client Side event convention.

## The basics

The extensions should use <a
href="https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">CustomEvent()</a> to talk to each
other and to Joomla! core. The event should be dispatched to modified
**Element** or to **window** (global)

The event name should have at least two part, separated ":", eg
**foo:bar**, **myshop:add-to-cart**. Where the first part is an "event
supporter", and the second part is the event name which happened. Which
is allow us to avoid possible collisions with another scripts and native
DOM events. All Joomla! events should start from \`joomla:\`.

Example telling that User added something to the shopping cart:

```javascript
    // Shop extension:
    window.dispatchEvent(new CustomEvent('myshop:add-to-cart', {
      detail: {sku:'productSku', amount:10, name: 'Foo Bar'}
      bubbles: true,
      cancelable: true
    });
```

Then 3rd script can listen to it:

```javascript
    window.addEventListener('myshop:add-to-cart', function(event) {
      console.log(event.detail)
    });
```

## List of events

### joomla:updated

Dispatch it over the changed container, after the content was changed
example via ajax. Joomla listen to this event to update or initialise
its scripts accordingly to new content (example in a subform field).

Example content update with AJAX:

```javascript
    var $someContainer = document.querySelector('.my-dinamic-container');
    fetch('/foobar/page')
        .then(function(res) {
            return res.text();
        })
        .then(function(data) {
            $someContainer.innerHTML = data;
            $someContainer.dispatchEvent(new CustomEvent('joomla:updated', {
              bubbles: true,
              cancelable: true
            })
        });
```

The extensions can listen to it also, example to do initialisation for
modified part of the page:

```javascript
    document.addEventListener('joomla:updated', function(event){
        var $elements = event.target.querySelectorAll('.foo-bar');
        // ... do something with $elements
    });
```
