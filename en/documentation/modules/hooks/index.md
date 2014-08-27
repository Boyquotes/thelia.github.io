---
layout: home
title: About Hooks - Modules
sidebar: plugin
lang: en
subnav: plugin_hooks_about
---

<div class="page-header">
    <h1>Hooks : <small>about</small></h1>
</div>

<div class="alert alert-warning">
<p>This is a functionality in development and not available in the current version of Thelia. This is planned for version 2.1</p>
</div>

When you create a module for Thelia, you might want it to interact with the site in 2 ways :

- The heart of Thelia to generate new behaviors, such as sending an email to the administrator when a product is no longer available. On Thelia, this functionality is managed by the Events Listener/Dispatcher.
- Changing the display of some area of pages. You can control this with **Hooks**

The **Hooks** are actually entry points in the templates in which the modules will insert their own code, add new features and change the appearance of the site.

Before Hooks, the only way to change the display of the site was to change the template of the site to include new pieces of code to interact with the module.
This solution was not ideal as it required some integration work and can be quite complex for beginners and could cause problems if the modified template was the default ²Thelia template and an update of this template is available.

With Hooks, integration becomes easier to see fully automatic. The modules using hooks provide a default implementation based on the default Thelia template. If you have created a template based on the default template then you should have nothing else to do than to activate the module.

The great strength of Thelia is the possibility to customize the display of the site. It is possible that some modules do not integrate in an optimal way with their default implementation.
Fortunately, a system of overriding is possible in the template, without having to touch module files, making an update of the module possible.


## How does it work

The principle of hook is simple. A smarty function or a smarty block is added in the template. When smarty parses this function/block an event is created and dispatched to methods of modules that listen to this hook event.

This event just grab contents generated by the chain of modules. When the event is finished, the content is injected in place of the function/block.

There is two distinct behaviors for managing hooks : 

### The hook function

The smarty function ```{hook name="hookname" ... }``` injects the code generated during the event propagation. *The fragments of HTML code generated in modules are simply concatenated*. 

These hooks use a ```Thelia\Core\Event\Hook\HookRenderEvent``` event.

#### Example of a hook function :

In this example, the hook code is ```product.details.top``` and the code generated by modules listening to this hook is concatenated and injected here.

*Notice: you can add arguments to the smarty function (or block) to help identify the context of the request. Here, we have the product id that allows you to perform test on this product and display informations related to the current product.*

```html
...
<section id="product-details">
    {hook name="product.details.top" product="{$ID}"}
...
</section>
```

### The hook block

the smarty block ```{hookblock name="hookname" ... }...{/hookblock}``` works with ```{hookfor rel="hookname" ... }{/hookfor}``` smarty block and allows you to loop on each fragments generated in modules. 

These fragments are just arrays (hash tables). The hookFor iterate thrue these fragments and map them to smarty variables that you can use in your template.  

This type of hook are useful when you want to respect the layout of the template and just add the relevant informations from your module. For example, if you have a list of block in the sidebar that have the same appearence (title bar, a content, a link) and want to add your block. You will have to    attach your module to this hook and add a fragment with title, content and link data.

These hooks use a ```Thelia\Core\Event\Hook\HookRenderBlockEvent``` event.

#### Example of a hook block :

The smarty block is a little bit more complex but more flexible. This hook provides a list of row, each row has a list of variables. It is used in conjunction with the ```hookfor``` block that allow you to iterate on each row and assign variables to smarty. The variables may be different depending on the hook. You have to refer to the documentation of that hook.


```html
...
<section id="product-tabs">
    {hookBlock name="product.additional" product="{product attr="id"}"}
    <ul class="nav nav-tabs" role="tablist">
        <li class="active" role="presentation"><a id="tab1" href="#description" data-toggle="tab" role="tab">{intl l="Description"}</a></li>
        {forHook rel="product.additional"}
            <li role="presentation"><a id="tab{$id}" href="#{$id}" data-toggle="tab" role="tab">{$title}</a></li>
        {/forHook}
    </ul>
    <div class="tab-content">
        <div class="tab-pane active in" id="description" itemprop="description" role="tabpanel" aria-labelledby="tab1">
            <p>{$DESCRIPTION|default:'N/A' nofilter}</p>
        </div>
        {forHook rel="product.additional"}
        <div class="tab-pane" id="{$id}" role="tabpanel" aria-labelledby="tab{$id}">
            {$content nofilter}
        </div>
        {/forHook}
    </div>
    {/hookBlock}
</section>
...
```

### Common behaviors between function and block

#### Atrributes

##### module

Some hooks are designed to be called for only a specific module, using the `module` attribute. In fact it's just used by delivery and paiement modules. Thus, you can call this hook for the delivery and the paiement module that have been selected for the order and not for all modules.

example : 

```html
{* delivery module can customize the delivery address *}
{hook name="order-invoice.delivery-address" module="{order attr="delivery_module"}"}
```

##### location

This argument exists only for retro compatibility with the module_include function used in the backOffice template. 

##### others

all other attributes that are used in the hook block/function will be added to the event propagated for this hook and could be used to precise the context of the hook (the id of the current product, ...)

### ifhook / elsehook

As the ifloop/elseloop, the hook has a ifhook/elsehook to test if any content has been generated. 

The behavior is similar and you have to use the `rel` attribute to specify the relative hook.
