# Access By Query String

A module for ProcessWire CMS/CMF. Grant/deny access to pages according to query string.

Allows visitors to view protected pages by accessing the page via a special URL containing an "access" GET variable. This allows you to provide a link to selected individuals while keeping the page(s) non-viewable to the public and search engines. The recipients of the link do not need to log in so it's very convenient for them.

The view protection does not provide a high level of security so should only be used for non-critical scenarios. The purpose of the module was to prevent new websites being publicly accessible before they are officially launched, hence the default message in the module config. But it could be used for selected pages on existing websites also.

Once a visitor has successfully accessed a protected page via the GET variable then they can view any other page protected by the same access rule without needing the GET variable for that browsing session.

Superusers are not affected by the module.

## Usage

[Install](http://modules.processwire.com/install-uninstall/) the Access By Query String module.

Define access rules in the format [GET variable]??[selector], one per line.

As an example the rule...

```
rumpelstiltskin??template=skills, title~=gold
```

...means that any pages using the "skills" template with the word "gold" in the title will not be viewable unless it is accessed with `?access=rumpelstiltskin` in the URL. So you could provide a view link like `https://domain.com/skills/spin-straw-into-gold/?access=rumpelstiltskin` to selected individuals.

Or you could limit view access to the whole frontend with a rule like...

```
4fU4ns7ZWXar??template!=admin
```

You can choose what happens when a protected page is visited without the required GET variable:

* Replace the rendered markup
* Throw a 404 exception

If replacing the rendered markup you can define a meta title and message to be shown. Or if you want to use more advanced markup you can hook `AccessByQueryString::replacementMarkup()`.

```php
$wire->addHookAfter('AccessByQueryString::replacementMarkup', function(HookEvent $event) {
    // Some info in hook arguments if needed...
    // The page that the visitor is trying to access
    $page = $event->arguments(0);
    // An array of access keys that apply to the page
    $access_keys = $event->arguments(1);

    // Return some markup
    $event->return = 'Your markup';
});
```

## Screenshot

![screenshot](https://user-images.githubusercontent.com/1538852/55221824-48ef4a80-526f-11e9-9529-efeffe21c4cd.png)