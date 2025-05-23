<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Using Bootstrap Components in Joomla 4 -->

## Introduction

### Bootstrap Components

Some of the components described in the Bootstrap documentation use CSS only. For example, the Breadcrumbs component is rendered with CSS and requires no JavaScript support. Others respond to user actions such as click or hover, and need JavaScript support. The latter are referred to here as Interactive Components. This article explains how to use them in Articles and a Custom Module.

See the [Bootstrap Documentation](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Joomla 4 Introduces a Modular Approach for Interactive Components

- What is a modular approach?
- The functionality is broken down into individual components supported by individual files. There is no **one file** approach as it was with Bootstrap in Joomla 3. The modular approach is more efficient and offers performance gains (send only the code that is needed instead of delivering everything in case some page will need some component).

### Using Interactive Components: Coders

- Load what you need per case! There are helper functions to set up individual components with appropriate arguments.
- See the list of Bootstrap Interactive Components.

### Using Interactive Components: Non-Coders

- Using components in articles is not so easy because the helper functions cannot be called from an article or standard Custom HTML module. Three possible solutions are suggested later in this tutorial:
  - A custom module
  - A custom plugin
  - A custom module override
- Skip or scan the list of Bootstrap Interactive Components. You won't be using these function calls directly.

## Bootstrap Interactive Components

### Alert

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- The **.selector** refers to the CSS selector for the alert. You can call this function multiple times with different CSS selectors.
- No extra options available

### Button

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- The **.selector** refers to the CSS selector for the button. You can call this function multiple times with different CSS selectors
- No extra options available

### Carousel

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- The **.selector** refers to the CSS selector for the carousel. You can call this function multiple times with different CSS selectors
- The third argument refers to the options available for carousel

Options for the carousel can be:

- **interval**, number, default:**5000**, The amount of time to delay between automatically cycling an item.
- **keyboard**, boolean, default:**true** Whether the carousel should react to keyboard events.
- **pause**, string\|boolean, **hover** If set to "hover", pauses the cycling of the carousel on mouseenter and resumes the cycling of the carousel on mouseleave. If set to false, hovering over the carousel won’t pause it. On touch-enabled devices, when set to "hover", cycling will pause on touchend (once the user finished interacting with the carousel) for two intervals, before automatically resuming. This is in addition to the mouse behavior.
- **ride**, string\|boolean, default:**false** If set to true, autoplays the carousel after the user manually cycles the first item. If set to "carousel", autoplays the carousel on load.
- **touch**, boolean, default:**true** Whether the carousel should support left/right swipe interactions on touchscreen devices.
- **wrap**, boolean, default:**true** Whether the carousel should cycle continuously or have hard stops.

### Collapse

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- The **.selector** refers to the CSS selector for the collapse. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for collapse.

Options for the collapse can be:

- **parent**, string, default:**false** If parent is provided, then all collapsible elements under the specified parent will be closed when this collapsible item is shown.
- **toggle**, boolean default:**true** Toggles the collapsible element on invocation.

### Dropdown

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- The **.selector** refers to the CSS selector for the dropdown. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for dropdown.

Options for the collapse can be:

- **flip**, boolean, default:**true** Allow Dropdown to flip in case of an overlapping on the reference element.
- **boundary**, string, default:**scrollParent** Overflow constraint boundary of the dropdown menu.
- **reference**, string, default:**toggle** Reference element of the dropdown menu. Accepts **toggle** or **parent**.
- **display**, string, default:**dynamic** By default, we use Popper for dynamic positioning. Disable this with **static**.

### Modal

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- The **.selector** refers to the CSS selector for the modal. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for modal.

Options for the modal can be:

- **backdrop**, string\|boolean default:**true** Includes a modal-backdrop element. Alternatively, specify static for a backdrop which doesn't close the modal on click.
- **keyboard**, boolean default:**true** Closes the modal when escape key is pressed.
- **focus**, boolean default:**true** Closes the modal when escape key is pressed.

### Offcanvas

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- The **.selector** refers to the CSS selector for the offcanvas. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for offcanvas.

Options for the offcanvas can be:

- **backdrop**, boolean, default:**true** Apply a backdrop on body while offcanvas is open.
- **keyboard**, boolean, default:**true** Closes the offcanvas when escape key is pressed.
- **scroll**, boolean, default:**false** Allow body scrolling while offcanvas is open.

### Popover

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- The **.selector** refers to the CSS selector for the popover. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for popover.

Options for the popover can be:

- **animation**, boolean, default:**true** Apply a CSS fade transition to the popover.
- **container**, string\|boolean, default:**false** Appends the popover to a specific element. Eg.: **body**.
- **content**, string, default:**null** Default content value if data-bs-content attribute isn't present.
- **delay**, number, default:**0** Delay showing and hiding the popover (ms) does not apply to manual trigger type.
- **html**, boolean, default:**true** Insert HTML into the popover. If **false**, innerText property will be used to insert content into the DOM.
- **placement**, string, default:**right** How to position the popover - **auto** \| **top** \| **bottom** \| **left** \| **right**. When auto is specified, it will dynamically reorient the popover.
- **selector**, string, default:**false** If a selector is provided, popover objects will be delegated to the specified targets.
- **template**, string, default:**null** Base HTML to use when creating the popover.
- **title**, string, default:**null** Default title value if **title** tag isn't present.
- **trigger**, string, default:**click** How popover is triggered - **click** \| **hover** \| **focus** \| **manual**.
- **offset**, integer, default:**0** Offset of the popover relative to its target.

### Scrollspy

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- The **.selector** refers to the CSS selector for the scrollspy. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for scrollspy.

Options for the Scrollspy can be:

- **offset** number Pixels to offset from top when calculating position of scroll.
- **method** string Finds which section the spied element is in.
- **target** string Specifies element to apply Scrollspy plugin.

### Tab

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- The **.selector** refers to the CSS selector for the tab. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for tab.

Options for the Tab can be:

### Tooltip

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- The **.selector** refers to the CSS selector for the tooltip. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for tooltip.

Options for the tooltip can be:

- **animation**, boolean apply a css fade transition to the popover.
- **container**, string\|boolean Appends the popover to a specific element: { container: **body** }.
- **delay**, number\|object delay showing and hiding the popover (ms) - does not apply to manual trigger type If a number is supplied, delay is applied to both hide/show Object structure is: delay: { show: 500, hide: 100 }.
- **html**, boolean Insert HTML into the popover. If false, jQuery's text method will be used to insert content into the dom.
- **placement**, string\|function how to position the popover - **top** \| **bottom** \| **left** \| **right**.
- **selector** string If a selector is provided, popover objects will be delegated to the specified targets.
- **template**, string Base HTML to use when creating the popover.
- **title**, string\|function default title value if **title** tag isn't present.
- **trigger**, string how popover is triggered - hover \| focus \| manual.
- **constraints**, array An array of constraints - passed through to Popper.
- **offset**, string Offset of the popover relative to its target.

### Toast

Assuming you have the HTML part already in your Layout, you will also need to include the interactivity (the JavaScript part):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- The **.selector** refers to the CSS selector for the toast. You can call this function multiple times with different CSS selectors.
- The third argument refers to the options available for toast.

Options for the toast can be:

- **animation**, boolean, default:**true** Apply a CSS fade transition to the toast.
- **autohide**, boolean, default:**true** Auto hide the toast.
- **delay**, number , default:**5000** Delay hiding the toast (ms).

### See Also

- **Accordian** uses Collapse to display panels of data.
- **List Group** can be combined with Tab to display tabbed content.

## Using Bootstrap Components in Articles

The HTML mark-up for most components can be included in an article or a module that can itself be included in an article. The snag is that the HTMLHelper call to set up the JavaScript support cannot be included there. There are several possible approaches to this problem. Three approaches are suggested here, using a custom Module, using a Plugin or using a Template override.

**Caution:** The TinyMCE and JCE editors remove white space on Save and make editing code difficult! The simple solution is to go to the top right of your Administrator screen and select **User Menu → Edit Account** and set the Editor to Code Mirror.

## Approach 1: Using a Custom Module

This is probably the least error prone approach because the Bootstrap component support options are selected with check boxes. The steps involved are as follows:

- Download, install and enable this [Custom Module](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip)
- From the Administrator menu go to **Content **→** Site Modules → New**
- Select **Custom BS Components**
- Enter a Title
- Toggle the Editor to plain text mode and paste in or type in the HTML code for the component you want to use.
- In the Options tab scroll down to the list of BS Components and select type of component in this instance of the module. Note that you can select more than one if your using more than one component.
- Select a module Position: either
  - a template defined position if you want to use the module in a specific location or
  - type in a position if you wish to use the module within a specific article: in the article type in {loadposition whatever}
- Save and go to the site to Test!

### Selectors

For some components JavaScript action is triggered by a specific **class** in the HTML code. In other components action is triggered by a **data-bs-whatever** attribute. The following are the current triggers and may change:

- **Alert** triggered by class="alert ..."
- **Button** triggered by data-bs-toggle="button"
- **Carousel** triggered by data-bs-ride="whatever"
- **Collapse** triggered by data-bs-toggle="collapse"
- **Dropdown** triggered by data-bs-toggle="dropdown"
- **Modal** triggered by data-bs-toggle="modal"
- **Offcanvas** triggered by data-bs-toggle="offcanvas"
- **Popover** triggered by class="btn ..." or
    - tag (could be changed to class="haspopover ...") AND
    - data-bs-toggle="popover"
- **Scrollspy** triggered by data-bs-spy="scroll"
- **Tab** triggered by data-bs-toggle="tab"
- **Toast** triggered by class="toast ..."
- **Tooltip** triggered by class="btn ..." or
    - tag (could be changed to class="hastooltip ...") AND
    - data-bs-toggle="tooltip"

### Example 1: Alert

Alerts may be used in html code without JavaScript support. This is only needed for the dismiss capability. HTML code example:

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Example result of including a module in an article:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-alert.png)

Note that without JavaScript support, the alert will appear exactly as above but a click on the close button \[X\] will not dismiss the alert. Also, the alert will appear on every page load.

### Example 2: Buttons

Buttons may be used in HTML code without JavaScript support. This is only needed for the sometimes subtle change of style applied to buttons with a change of state, styled active. Bootstrap example code:

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

With this style in the template user.css:

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap buttons](../../../en/images/coding-examples/coding-examples-buttons.png)

The buttons toggle between blue and green.

### Example 3: Carousel

The Carousel offers a slide show cycling through a series of images or text panes. The following example used images from the Joomla 4 Sample. Bootstrap code:

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Result:

![Bootstrap carousel](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Example 4: Collapse

Collapse is widely used in Joomla and you may not need to use a module or plugin to trigger action. The click opens a pane with extra information. Example Bootstrap code:

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Result:

![Bootstrap collapse](../../../en/images/coding-examples/coding-examples-collapse.png)

### Example 5: Dropdown

Dropdowns are toggleable, contextual overlays for displaying lists of links and more. Example Bootstrap code:

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Result:

![Bootstrap dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Example 6: Modal

The Modal component opens a dialog box in the middle of the screen. There are quite a few options to control the size and content of the modal. See the Bootstrap documentation for more details. Example Bootstrap code:

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Result:

![Bootstrap modal](../../../en/images/coding-examples/coding-examples-modal.png)

### Example 7: Offcanvas

At the moment this component is not supported in Joomla. Watch this space - coming soon!

### Example 8: Popover

Popovers are like Tooltips but with a Title. They have some accessibility and performance issues so should be used with caution. Example Bootstrap code:

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Result:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-popover.png)

### Example 9: Scrollspy

Example code:

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Result:

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

Also, some styling is needed in user.css:

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

Snag: the menu does not coordinate well with the content in this example!

### Example 10: Tab

Tabs are often used as navigation elements combined with dropdowns. Bootstrap example code:

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Result:

![Bootstrap tab](../../../en/images/coding-examples/coding-examples-tab.png)

Remember to check both the Tab and Dropdown options for the dropdown part to work.

### Example 11: Toast

Toasts are lightweight notifications designed to mimic the push notifications that have been popularized by mobile and desktop operating systems. They’re built with flexbox, so they’re easy to align and position. Example Bootstrap code:

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Result:

![Bootstrap toast](../../../en/images/coding-examples/coding-examples-toast.png)

Note that the Bootstrap demo that uses a button to show the Toast message needs some extra JavaScript. It seems this component needs a coder to make good use of it!

### Example 12: Tooltip

A tooltip is a small piece of text that appears on hover over a button or link element to explain what it is or does. The tooltip can be positioned above or below or to the left or right of the element. If not specified the default position is top. The tooltip will switch to another position if there is insufficient room in the specified position. Example Bootstrap code:

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Result:

![Bootstrap tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Approach 2: Using a Content Plugin

The steps involved:

- Download, install and enable this [plugin](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- In the article add the text that the plugin acts on, for example {bscompos modal carousel} will trigger loading of the JavaScript necessary to support a modal dialog and a carousel. The plugin removes the trigger text and enclosing (now) empty tags.
- Include the Bootstrap Component HTML code directly in the article or in a module included in the article. There is example HTML code below for a simple Modal and a Modal containing a Carousel. Note that this will not work if the HTML code is in a module in a template location.
- This will also work for a standard Custom module if the Prepare Content Option is set to Yes.
- Test it!

## Approach 3: Using a Template Override

The steps involved:

- Create a mod_custom template override.
- Add a mod_custom module containing the component markup and trigger classes.
- Include the module in an article.

### The mod_custom Template Override

- In the Administrator interface go to **System → Site Templates → Cassiopeia Details and Files**.
- Select **Create Overrides **→** mod_custom **→** default.php**.
- On the line following defined('\_JEXEC') or die; add the following code:

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

This code searches for class names set in mod_custom and makes the HTMLHelper call to set up the JavaScript support. Note that the last item in each call is a selector that may or may not be used to trigger action. Many of the components are triggered by data attributes in the mark-up and they do not use the selectors here. For some, the selector is needed. For example, it makes sense to use the **.btn** class and the **a** tag to trigger Tooltips.

### A mod_custom Module for a Modal Component

- Go to **Content **→** Site Modules **→** New**.
- Select the **Custom** module.
- Fill out the form:
  - Title: Demo Modal
  - In the Position field type in **demomodal** for use in an article;
  - Module Content: Toggle Editor for plain text entry.
  - Paste in the following code from the Bootstrap documentation:

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Select the Advanced tab and in the Module Class field enter **bs-modal**
- Optional: set Ttitle to Hide to use the H2 in the pasted code.
- Save and Close (do not worry that the modal looks all wrong in the editor).

### Create an Article and Menu Item

- Create a new article, Demo Modal, and in plain text entry mode set the content to

```html
<div>{loadposition demomodal}</div>
```

- Create a Single Article menu item.
- Test it:

![Bootstrap modal module in article](../../../en/images/coding-examples/coding-examples-modal-module.png)

### A Modal Component Containing a Carousel

- Create a new Custom module with a new Title: Demo Modal Carousel
- Position: demomodalcarousel
- **Advance tab **→** Module Class**: bs-modal bs-carousel
- Module Custom content in plain text:

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Create a new Article with {loadposition demomodalcarousel} in the content.
- Create a new single article menu item: Demo Modal Carousel
- Test it:

![Bootstrap modal carousel](../../../en/images/coding-examples/coding-examples-modal-carousel.png)
