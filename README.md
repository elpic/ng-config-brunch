# ng-config-brunch

Generates an angular module from a json config file

## Usage

Install the plugin via npm with `npm install git@github.com:pankratios/ng-config-brunch.git`.

## Options

Import external data through the `imports` section.

```javascript
{
  ...
  plugins: {
    ngconfig: {
      imports: [
        './package.json',
        './bower.json'
      ]
    }
  }
}
```

The imported data can be accessed with `{{...}}` notation.

```javascript
{
  "packageVersion": "{{=it.package.version}}"
  "bowerVersion": "{{=it.bower.version}}"
}
```

## Configuration file structure

Configurations can be overriden per `environment`, `development` is the default.

This files can be placed on the app folder and work fine, but if you want to add a
directory on the root of the project make sure to include that folder to the ones
that brunch watch, to do that you can modify you brunch config like this:

```javascript
exports.config = {

  // More configuration options

  "paths": {
    "watched": ['app', folder_to_include, 'test', 'vendor']
  }

  // More configuration options

}
```

## Example

`*.conf.json`
```javascript
{
  "moduleName": "myApp.config",
  "values": {
    "appName": "App name is {{=it.bower.name}}",
  },
  "constants": {
    "appVersion": "version {{=it.package.version}}",
    "requestTimeout": 3000
  },
  "overrides": {
    "production": {
      "constants": {
        "requestTimeout": 1000
      }
    }
  }
}
```

### Generated module

`development env`
```javascript
angular
  .module("myApp.config", [])
  .constant("appVersion", "version 1.0.9")
  .value("appName", "App name is angular-config-example")
  .constant("requestTimeout", 3000)
```

`production env`
```javascript
angular
  .module("myApp.config", [])
  ...
  .constant("requestTimeout", 1000)
```
