# Custom config parameters

In this article it will be shown how to add custom config parameters and how to make them available on the administration panel.

In the example it's supposed that you module is named `MyModule`. Replace `MyModule` and `my-module` with your real module name.

### 1. Metadata entityDefs > settings

Create a file `custom/Espo/Modules/MyModule/Resources/metadata/entityDefs/Settings.json`:

```json
{
    "fields": {
        "myParameter": {
            "type": "enum",
            "options": ["Option 1", "Option 2"],
            "tooltip": true
        }
    }
}
```

### 2. Metadata app > config

Create a file `custom/Espo/Modules/MyModule/Resources/metadata/app/config.json`:

```json
{
    "params": {
        "myParameter": {
            "level": "admin"
        }
    }
}
```

Setting the *level* parameter to `"admin"` make the parameter available only for admin users. Available values: `global`, `system`, `admin`, `superAdmin`.

### 3. Metadata app > adminPanel

Create a file `custom/Espo/Modules/MyModule/Resources/metadata/app/adminPanel.json`:

```json
{
    "myPanel": {
        "label": "My Panel",
        "itemList": [
            {
                "url": "#Admin/mySettings",
                "label": "My Settings",
                "iconClass": "fas fa-cog",
                "description": "myPanel",
                "recordView": "my-module:views/admin/my-settings"
            }
        ],
        "order": 101
    }
}
```

### 4. Language files

Create a file `custom/Espo/Modules/MyModule/Resources/i18n/en_US/Admin.json`:

```json
{
    "keywords": {
        "myPanel": "some search keyword,another search keyword"
    },
    "descriptions": {
        "myPanel": "Description text for my panel."
    },
    "labels": {
        "My Settings": "My Settings",
        "My Panel": "My Panel"
    }
}
```


Create a file `custom/Espo/Modules/MyModule/Resources/i18n/en_US/Settings.json`:

```json
{
    "fields": {
        "myParameter": "My Parameter"
    },
    "tooltips": {
        "myParameter": "My parameter tooltip text. Markdown supported."
    },
    "options": {
        "myParameter": {
            "Option 1": "Option 1",
            "Option 2": "Option 2"
        }
    }
}
```

### 5. View

Create a file `client/custom/modules/my-module/src/views/my-settings.js`:

```js
define('my-module:views/admin/my-settings', 'views/settings/record/edit', function (Dep) {

    return Dep.extend({

        detailLayout: [
            {
                rows: [
                    [
                        {
                            name: 'myParameter'
                        },
                        false
                    ]
                ]
            }
        ],

        // Dynamic logic can de defined.
        dynamicLogicDefs: {},

        setup: function () {
            Dep.prototype.setup.call(this);
            
            // Some custom logic can be written here.
        },
    });
});
```
### 6. Final step

Clear cache.

The parameter value now can be read this way:

```php
$this->config->get('myParameter');
```