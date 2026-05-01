# PHPNomad WordPress Plugin Starter

Scaffold a brand-new PHPNomad WordPress plugin in two commands.

## Usage

```bash
composer create-project phpnomad/wp-plugin-starter my-plugin
cd my-plugin
vendor/bin/phpnomad make --from=phpnomad/wp-plugin '{
  "pluginName": "My Plugin",
  "description": "What my plugin does.",
  "vendor": "acme",
  "package": "my-plugin",
  "namespace": "AcmeMyPlugin",
  "textDomain": "my-plugin",
  "authorName": "Your Name",
  "authorEmail": "you@example.com",
  "authorUrl": ""
}'
```

That's it. After the recipe runs, you have a working PHPNomad WordPress plugin with the right entry file, Application class, root initializer, autoload, tests, and `composer.json` rewritten to match your plugin's identity.

### Notes on the bootstrap arguments

- `namespace` — single-segment, PascalCase, no backslashes (e.g. `AcmeMyPlugin`, not `Acme\\MyPlugin`). v1 of the bootstrap recipe doesn't support nested namespaces; if you need one, edit `composer.json` after scaffolding.
- `authorUrl` — pass an empty string if you don't have one. WordPress will still display the plugin without an Author URI line.

From there, use the rest of PHPNomad's recipes to add functionality. The `initializer` arg points at `{namespace}\\AppInit` — the root initializer the bootstrap created:

```bash
vendor/bin/phpnomad make --from=phpnomad/datastore '{"name":"Order","initializer":"AcmeMyPlugin\\\\AppInit"}'
vendor/bin/phpnomad make --from=phpnomad/cpt-datastore '{"name":"Event","postType":"acme-event","pluralLabel":"Events","singularLabel":"Event","textDomain":"my-plugin","initializer":"AcmeMyPlugin\\\\AppInit"}'
```

Run `vendor/bin/phpnomad recipes:list --all=1` to see everything available across the installed kits.

## Why a starter plus recipes (and not a clone-this-template)

This starter is intentionally tiny. Its only job is to give you `phpnomad/cli` and `phpnomad/wp-plugin-recipes` in your project so the bootstrap recipe can run. The actual plugin structure lives in versioned recipes that improve and ship independently. If you cloned this starter today and again in six months, the second project benefits from improvements without you copying anything.

The previous template approach (`phpnomad/wp-plugin-template`) is being retired in favor of this pattern.

## What you get after the bootstrap

```
my-plugin/
├── my-plugin.php           # WordPress plugin entry file with proper plugin header
├── lib/
│   ├── Application.php     # PHPNomad Application class wired for WordPress
│   └── AppInit.php         # Root initializer — recipes register against this
├── tests/
│   └── bootstrap.php       # Minimal PHPUnit bootstrap
├── composer.json           # Rewritten with your plugin's identity and full deps
├── phpunit.xml
├── .gitignore
├── .phpnomad/
│   └── config.json         # Active recipe set (defaults to all installed)
└── README.md               # Your plugin's README
```

## Requirements

- PHP 8.2 or higher
- Composer 2.0 or higher
- WordPress 6.0 or higher (when activated as a plugin)

## License

MIT
