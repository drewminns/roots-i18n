# roots-i18n
A naive i18n extension for the [Roots](http://roots.cx/) static site generator ecosystem

This extension makes many assumptions. It is not (yet) a catch-all solution to i18n in Roots. However, within strict limitations, it can be considered _production ready_.

## Installation

```sh
$ npm i --save-dev roots-i18n
```

## Usage

**roots-i18n** requires that your translations are saved as a local file in [yaml](http://yaml.org/) format. Each language should be in a separate file. Your folder structure might look something like this:

```
root
 ├─ views
 ├─ assets
 ├─ i18n
 │   ├─ en.yaml
 │   └─ es.yaml
 ├─ app.coffee
 └─ ...
```

Each language file should be named using the 2-letter language code, as above. The contents of `es.yaml` might look like this:

```yaml
dir: es/
lang: es

homepage:
  message: empezar aquí y elegir una categoría

login:
  title: iniciar sesión / inscribirse

faq:
  title: Preguntas Frecuentes

dashboard:
  title: Tu cuadro de mandos

  tabs:
    requests: Solicitudes
    quotes: Presupuestos
    jobs: Trabajos
    completed: Terminados

messaging:
  title: Mensajes
```

Those first 2 lines are not required for **roots-i18n** to work, but they can be useful in your view templates, [explained below](#). `dir` is the name of the output subdirectory where the Spanish (for example) version of your website will be located.

The `lang` property is simply the 2-letter language code, the same as the filename.

Add **roots-i18n** to your `extensions` array as follows:

```coffeescript
i18n = require 'roots-i18n'

module.exports =
  extensions: [
    # ...

    i18n(
      translations: 'i18n/*'
      viewExtension: 'jade'
      templatesGlob: 'request/**/*.html'
    )

    # ...
  ]
```

**roots-i18n** takes a configuration object with the following properties:

- `translations`: This is a globbing pattern that matches all your translation yaml files, [explained above](#Usage).
- `viewExtension`: This tells **roots-i18n** how to identify your view templates.
- `templatesGlob`: This property exists to play nicely with [roots-records](https://github.com/carrot/roots-records). If you're not using that extension, you can omit this property. If you are, this should be a globbing pattern that identifies all your **roots-records** ["single view templates"](https://github.com/carrot/roots-records#template-and-out).

**roots-i18n** assumes that the default language of your site is English, and places translated versions of the site in subdirectories named after the 2-letter language code, i.e. a Spanish homepage would be at `example.com/es/`, with the English homepage at `example.com/`. It only handles view templates – assets are untouched. **roots-i18n** assumes that all assets are non-language-specific, and are accessed at the root directory of your site.
