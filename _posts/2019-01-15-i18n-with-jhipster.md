---
layout: single
title:  "I18N with JHipster"
date:   2019-01-15 19:24:21 +0100
excerpt: "JHipster supports internationalization, but how does it work exactly? And what about my custom messages?"
categories: [tech]
tags: [JHipster, I18N]
---
I myself am a huge fan of generators - it saves a lot of time. That's why I love [JHipster](http://http://jhipster.tech). JHipster not only uses my favourite frameworks such as Spring Boot and Angular, but it provides you with useful features out-of-the-box.

One of the questions of the wizard guiding you through the generation of your application, is which languages you want to support. For every selected language, all possible labels are generated.

But what about *your* labels in *your* custom developed component? Let's have a look.

First you need the actual translations. They are located in `src/main/webapp/i18n`. For each language you selected in the wizard, there is a subfolder named according to a [ISO standard](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes): English translations are stored in the `en`folder, Dutch in `nl`, ...

These folder contain json files - one for each JHipster entity, and some utility files (login screen, settings, error messages, ...). Next are the labels for error messages in Dutch:

```json
{
    "error": {
        "title": "Foutpagina!",
        "http": {
            "400": "Bad request.",
            "403": "U bent niet bevoegd om de pagina te openen.",
            "405": "The HTTP verb you used is not supported for this URL.",
            "500": "Internal server error."
        },
        "concurrencyFailure": "Another user modified this data at the same time as you. Your changes were rejected.",
        "validation": "Validation error on the server."
    }
}
```

So if you want to display the message for an unauthorized access, you use the label ``error.http.403``.

But how do you actually display the translated message in your angular component? With a directive off course, courtesy of JHipster. In the next snippet only the relevant parts remain (so no ``[hidden]`` to only display in case of a 403 error)

```html
<div class="alert alert-danger" jhiTranslate="error.http.403">You are not authorized to access this page.</div>
```

So you use the ``jhiTranslate`` directive, assign it the label key, as you defined it in the huge json object in the translation file. The actual value of the div is the fallback message in case no translation is available.

Final question to answer: how do I configure to include my custom translation files? You don't :) Just check ``webpack/webpack.common.js``.

```javascript
new MergeJsonWebpackPlugin({
            output: {
                groupBy: [
                    { pattern: "./src/main/webapp/i18n/nl/*.json", fileName: "./i18n/nl.json" }
                    // jhipster-needle-i18n-language-webpack - JHipster will add/remove languages in this array
                ]
            }
        })
```

It contains a ``MergeJsonWebpackPlugin``, you supply it with a pattern of the files to merge and an output file. And this particular pattern is in case of Dutch ``./src/main/webapp/i18n/nl/*.json``. So it includes **all** json files in the directory. And that's why your new, custom translation is automatically available after a rebuild.

Now go [translate](http://translate.google.com) 🙂.
