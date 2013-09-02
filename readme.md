Maintainer: [Mike Pearson](https://github.com/gmp26)

Based on [angular-generator](https://github.com/yeoman/angular-generator/)

This generator adds the following options

* [LiveScript](http://livescript.net) as an alternative to Javascript or Coffeescript,
* Bootstrap with Less/Recess as an alternative to Sass/Compass. This is useful if you 
  want to use angular-ui/bootstrap,
* Font-awesome instead of glyphicons.

It is tested most extensively with these
options enabled.


## Provisional Usage

NB: Until things stabilise further I won't publish this as a node module, so to use you will
have to git clone the repo, cd to it, and run `npm link`.
The generator will then be available globally so skip the install step.

Install `generator-angular-ls`:
```
npm install -g generator-angular-ls
```

Make a new directory, and `cd` into it:
```
mkdir my-new-project && cd $_
```

Run `yo angular-ls`, optionally passing an app name. If you want coffeescript or livescript, also
pass the --coffee or the --ls language flag.
```
yo angular-ls [app-name] [(--coffee)|(--ls)]
```

## Generators

Available generators:

* [angular-ls](#app) (aka [angular-ls:app](#app))
* [angular-ls:controller](#controller)
* [angular-ls:directive](#directive)
* [angular-ls:filter](#filter)
* [angular-ls:route](#route)
* [angular-ls:service](#service)
* [angular-ls:decorator] (#decorator) - tests fail on this. If you know why, please send a pull request.
* [angular-ls:view](#view)

**Note: Generators are to be run from the root directory of your app.**

### App
Sets up a new AngularJS app, generating all the boilerplate you need to get started. The app generator also optionally installs Twitter Bootstrap and additional AngularJS modules, such as angular-resource.

The chosen language will determine the default language used for subsequent generators like angular-ls:controller.

Example:
```bash
yo angular-ls --ls
```

### Route
Generates a controller and view, and configures a route in `app/scripts/app.js` connecting them.

Example:
```bash
yo angular-ls:route myroute
```

Produces `app/scripts/controllers/myroute.js`:
```javascript
angular.module('myMod').controller('MyrouteCtrl', function ($scope) {
  // ...
});
```

Produces `app/views/myroute.html`:
```html
<p>This is the myroute view</p>
```

### Controller
Generates a controller in `app/scripts/controllers`.

Example:
```bash
yo angular-ls:controller user
```

Produces `app/scripts/controllers/user.js`:
```javascript
angular.module('myMod').controller('UserCtrl', function ($scope) {
  // ...
});
```
### Directive
Generates a directive in `app/scripts/directives`.

Example:
```bash
yo angular-ls:directive myDirective
```

Produces `app/scripts/directives/myDirective.js`:
```javascript
angular.module('myMod').directive('myDirective', function () {
  return {
    template: '<div></div>',
    restrict: 'E',
    link: function postLink(scope, element, attrs) {
      element.text('this is the myDirective directive');
    }
  };
});
```

### Filter
Generates a filter in `app/scripts/filters`.

Example:
```bash
yo angular-ls:filter myFilter
```

Produces `app/scripts/filters/myFilter.js`:
```javascript
angular.module('myMod').filter('myFilter', function () {
  return function (input) {
    return 'myFilter filter:' + input;
  };
});
```

### View
Generates an HTML view file in `app/views`.

Example:
```bash
yo angular-ls:view user
```

Produces `app/views/user.html`:
```html
<p>This is the user view</p>
```

### Service
Generates an AngularJS service.

Example:
```bash
yo angular-ls:service myService
```

Produces `app/scripts/services/myService.js`:
```javascript
angular.module('myMod').service('myService', function () {
  // ...
});
```

You can also do `yo angular:factory`, `yo angular:provider`, `yo angular:value`, and `yo angular:constant` for other types of services.

### Decorator

THIS IS NOT WORKING YET - tests fail. If you know why, please send a pull request.

Generates an AngularJS service decorator.

Example:
```bash
yo angular-ls:decorator serviceName
```

Produces `app/scripts/decorators/serviceNameDecorator.js`:
```javascript
angular.module('myMod').config(function ($provide) {
    $provide.decorator('serviceName', function ($delegate) {
      // ...
      return $delegate;
    });
  });
```

## Options
In general, these options can be applied to any generator, though they only affect generators that produce scripts.

### CoffeeScript and LiveScript
For generators that output scripts, the `--coffee` or `--ls` option will output CoffeeScript or LiveScript instead of JavaScript. The dafult language is determined by the initial yo run.

For example:
```bash
yo angular:controller user --coffee
```

Produces `app/scripts/controller/user.coffee`:
```coffeescript
angular.module('myMod')
  .controller 'UserCtrl', ($scope) ->
```

A project can mix CoffeScript and JavaScript files.

### Minification Safe
By default, generators produce unannotated code. Without annotations, AngularJS's DI system will break when minified. Typically, these annotations the make minification safe are added automatically at build-time, after application files are concatenated, but before they are minified. By providing the `--minsafe` option, the code generated will out-of-the-box be ready for minification. The trade-off is between amount of boilerplate, and build process complexity.

#### Example
```bash
yo angular-ls:controller user --minsafe
```

Produces `app/controller/user.js`:
```javascript
angular.module('myMod').controller('UserCtrl', ['$scope', function ($scope) {
  // ...
}]);
```

#### Background
Unannotated:
```javascript
angular.module('myMod').controller('MyCtrl', function ($scope, $http, myService) {
  // ...
});
```

Annotated:
```javascript
angular.module('myMod').controller('MyCtrl',
  ['$scope', '$http', 'myService', function ($scope, $http, myService) {

    // ...
  }]);
```

The annotations are important because minified code will rename variables, making it impossible for AngularJS to infer module names based solely on function parameters.

The recommended build process uses `ngmin`, a tool that automatically adds these annotations. However, if you'd rather not use `ngmin`, you have to add these annotations manually yourself.

## Bower Components

The following packages are always installed by the [app](#app) generator:

* angular
* angular-mocks
* angular-scenario


The following additional modules are available as components on bower, and installable via `bower install`:

* angular-cookies
* angular-loader
* angular-resource
* angular-sanitize

All of these can be updated with `bower update` as new versions of AngularJS are released.

## Configuration
Yeoman generated projects can be further tweaked according to your needs by modifying project files appropriately.

### Output
You can change the `app` directory by adding a `appPath` property to `bower.json`. For instance, if you wanted to easily integrate with Express.js, you could add the following:

```json
{
  "name": "yo-test",
  "version": "0.0.0",
  ...
  "appPath": "public"
}

```
This will cause Yeoman-generated client-side files to be placed in `public`.

## Testing

For tests to work properly, karma needs the `angular-mocks` bower package.
This script is included in the bower.json in the `devDependencies` section, which will
be available very soon, probably with the next minor release of bower.

While bower `devDependencies` are not yet implemented, you can fix it by running:
```bash
bower install angular-mocks
```

By running `grunt test` you should now be able to run your unit tests with karma.

## Contribute

See the [contributing docs](https://github.com/yeoman/yeoman/blob/master/contributing.md)

When submitting an issue, please follow the [guidelines](https://github.com/yeoman/yeoman/blob/master/contributing.md#issue-submission). Especially important is to make sure Yeoman is up-to-date, and providing the command or commands that cause the issue.

When submitting a PR, make sure that the commit messages match the [AngularJS conventions](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/).

When submitting a bugfix, write a test that exposes the bug and fails before applying your fix. Submit the test alongside the fix.

When submitting a new feature, add tests that cover the feature.

## License

[BSD license](http://opensource.org/licenses/bsd-license.php)
