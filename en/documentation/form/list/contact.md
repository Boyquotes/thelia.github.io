---
layout: form
title: Contact
form_name: thelia.front.contact
sidebar: form
subnav: form_list_contact
fields:
    - { name: "name", mandatory: "true", description: "name of the customer who is contacting you"}
    - { name: "email", mandatory: "true", description: "email of the customer who is contacting you"}
    - { name: "subject", mandatory: "true", description: "Subject of the message"}
    - { name: "message", mandatory: "true", description: "content of the message"}

lang: en

---
```
{form name="thelia.front.contact"}
<form id="form-contact" action="{url path="/contact"}" method="post">
    {form_hidden_fields form=$form}
    <fieldset id="contact-info" class="panel">
        <div class="panel-heading">
            {intl l="Send us a message"}
        </div>
        <div class="panel-body">
            <div class="row">
            {form_field form=$form field="name"}
                <div class="form-group group-name col-sm-6{if $error} has-error{/if}">
                    <label class="control-label" for="{$label_attr.for}">{$label}{if $required} <span class="required">*</span>{/if}</label>
                    <div class="control-input">
                        <input type="text" name="{$name}" id="{$label_attr.for}" class="form-control" maxlength="255" placeholder="{intl l="Placeholder contact name"}" value="{$value}"{if $required} aria-required="true" required{/if}{if !isset($error_focus) && $error} autofocus{/if}>
                        {if $error }
                            <span class="help-block">{$message}</span>
                            {assign var="error_focus" value="true"}
                        {elseif $value != "" && !$error}
                            <span class="help-block"><i class="icon-ok"></i></span>
                        {/if}
                    </div>
                </div><!--/.form-group-->
            {/form_field}
            {form_field form=$form field="email"}
                <div class="form-group group-email col-sm-6{if $error} has-error{/if}">
                    <label class="control-label" for="{$label_attr.for}">{$label}{if $required} <span class="required">*</span>{/if}</label>
                    <div class="control-input">
                        <input type="email" name="{$name}" id="{$label_attr.for}" class="form-control" maxlength="255" placeholder="{intl l="Placeholder contact email"}" value="{$value}"{if $required} aria-required="true" required{/if}{if !isset($error_focus) && $error} autofocus{/if}>
                        {if $error }
                            <span class="help-block">{$message}</span>
                            {assign var="error_focus" value="true"}
                        {/if}
                    </div>
                </div><!--/.form-group-->
            {/form_field}
            </div>
            {form_field form=$form field="subject"}
                <div class="form-group group-firstname{if $error} has-error{/if}">
                    <label class="control-label" for="{$label_attr.for}">{$label}{if $required} <span class="required">*</span>{/if}</label>
                    <div class="control-input">
                        <input type="text" name="{$name}" id="{$label_attr.for}" class="form-control" maxlength="255" placeholder="{intl l="Placeholder contact subject"}" value="{$value}" {if $required} aria-required="true" required{/if}{if !isset($error_focus) && $error} autofocus{/if}>
                        {if $error }
                            <span class="help-block">{$message}</span>
                            {assign var="error_focus" value="true"}
                        {/if}
                    </div>
                </div><!--/.form-group-->
            {/form_field}
            {form_field form=$form field="message"}
                <div class="form-group group-message{if $error} has-error{/if}">
                    <label class="control-label" for="{$label_attr.for}">{$label}{if $required} <span class="required">*</span>{/if}</label>
                    <div class="control-input">
                        <textarea name="{$name}" id="{$label_attr.for}" placeholder="{intl l="Placeholder contact message"}" rows="6" class="form-control"{if $required} aria-required="true" required{/if}>{$value}</textarea>
                        {if $error }
                            <span class="help-block">{$message}</span>
                            {assign var="error_focus" value="true"}
                        {/if}
                    </div>
                </div><!--/.form-group-->
            {/form_field}

            <div class="form-group group-btn">
                <div class="control-btn">
                    <button type="submit" class="btn btn-contact">{intl l="Send"}</button>
                </div>
            </div><!--/.form-group-->
        </div>
    </fieldset>
</form>
{/form}
```