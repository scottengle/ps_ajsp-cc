# Course Work for Pluralsight Course 'AngularJS Patterns: Clean Code'

https://app.pluralsight.com/library/courses/angularjs-patterns-clean-code

# Sample Code

https://github.com/johnpapa/ng-demos

# Style Guide

https://github.com/johnpapa/angular-styleguide/

# Running The Samples

* Run mongo 
  * `docker run mongo`
* Go to `modular` folder
  * `cd modular`
* Install app
  * npm i
* Run app
  * `gulp serve-dev`
* Load app in browser
  * http://localhost:7203

# Separation of Concerns (SoC)

## Benefits

* Easier to maintain
* Easier to extend
* Reuse more code
* Easier to test

SoC can be equated to "Ravioli Code", small self-contained modules.

## Rule of 1

* A component has one role
* One component per file
* Singular Purpose

Tips to help follow the Rule of 1

* Create small, readable and singularly focused components
  * Controllers defer to factories
  * Value Proposition
    * Lack of duplication makes it easier to maintain
    * Increase stability
    * Easier to extend and enhance
    * More reuse by other components
* Module startup logic is difficult to test
  * Inject and invoke module startup code
* Take advantage of modularity
  * Group features into modules

## SoC Examples

* Cross Cutting / Specific to the App
  * Managing XHR calls to send/receive data (i.e. a data service)
* Cross Cutting / Generic
  * Logging Service
  * Exception Handling consolidation
* Feature / Role
  * Customer controller
  * Customer address widget (via a directive)

## Summary

* 1 Component, 1 Role, 1 File
* Use Dependencies
* Modularize
* Decreases risk, cost, time to deliver
* Increases testability, code reuse, maintainability

# Organizing The App

## Benefits of a Solid Structure

* Faster and easier to develop
* Aligns with SoC
* Easier to find and locate code
* Transitions to modularity when its time to extend

## By Type of By Feature?

Prefer "By Feature" organization for apps. Features can evolve naturally into modules.

## The LIFT Principle

* `L`ocating our code is easy
* `I`dentify the code at a glance
* `F`lat structure as long as we can
* `T`ry to say DRY (Don't Repeat Yourself!)

## Naming Conventions

* Be consistent
* Make it immediately identifiable (refer to LIFT)
* Agree on a convention with your team and company
* Use the AngularJS Style Guide: https://github.com/johnpapa/angular-styleguide/

Specifics

* Add `<name>.module.js` to module file names
* Don't suffix Controllers with `controller`
* Use Pascal casing for Controller names, camelCasing for other types of components
* Name a factory the same as the file name, without suffix
* Directive file names and names should match, with one exception
* Prefix the directive name with a two- or three- letter code
  * i.e. `ccWidgetHeader`
* Choose descriptive names for components
* Avoid generic names like utility
* Use dots, dashes or camelCasing for multi-word file names
* Be consistent!

## Summary

* Follow the LIFT principle
* Start with the smallest thing that makes sense
* When in doubt, organize by feature
* Be consistent with your team!

# Modules

Separate your app into reusable components. Modular apps allow you to plug features in over time.

* Each module can be an app, services or widgets (or all of these)
* Modules can depend on other modules
* Modules promote continuous development

*Modules are containers for AngularJS components*

Module hierarchy

* Module
  * Config
    * Routes
  * Filter
  * Directive
  * Providers
    * Factory
    * Service
    * Provider
    * Value
    * Constant
  * Controller

Created using `angular.module()`

    // Setter
    angular.module('app', []);

    // Getter
    angular.module('app');

## Three categories

* AngularJS Modules
* 3rd Party Modules
* Custom Modules

Modules may rely on functionality from other modules. Helper modules can be "injected" into a module.

    angular.module('app', [
      // Angular modules
      'ngRoute',
      
      // Custom modules
      'app.dashboard',

      // 3rd Party Modules
      'ui.bootstrap',
      'breeze.angular'
    ]);

Separate applications by features, using modules.

## Organization Strategies

Define dependencies for each module, in each module.

* Will always work
* Fairly common
* May be difficult to follow

Define dependencies at specific levels

* Requires some forethought
* Easier to maintain
* Main App module
  * runs entire app
  * has little or no functionality
  * aggregates feature area modules
* Feature modules
  * encapsulates a set of features
  * development team can work on these separately
  * incrementally add or remove as needed
  * can add dependencies here, or in Main App module
* App Sharable Modules
  * Aggregates all AngularJS, 3rd party and app generic custom modules
  * Examples
    * Core
    * Data
    * Widgets

## Who Wins with Collisions

Last module loaded wins. If you have two modules or components named the same, you will get unexpected results.

## Naming Tips

* Name modules consistently
  * app is the main modules
  * app.avengers is a feature module
* Name files consistently
  * app.js (or app.module.js)
  * avengers.module.js

## Summary

* Modules are feature areas
* Modules help keep features encapsulated
* Modules reinforce SoC
* Don't create a spider web of module dependency chains
* Aggregate where it makes sense
* Good debugging skills can be helpful

# Readable Code

* Module Variables or Chaining
  * Choose chaining
  * `angular.module('app', []).controller('App')`
* Anonymous or Named Functions
  * Use named functions and take advantage of hoisting
* IIFE
  * Always use an IIFE
* Safe Minify
  * Use an inline dependency array
  * `angular.module('app').controller('Dashboard', ['common', 'dataservice', Dashboard])`
* Register, Inject, then Function
  * using ngAnnotate: `/* @ngInject */` (prefered)
  * using $inject: `SessionDetail.$inject = [...]`
* Reading Component Interfaces
  * Keep the interface definition near the top of the code

## Summary

* IIFEs help reduce global variables and functions
* Chaining and indentation
* Named functions help readability
* Keep important code up top including interfaces
* Inject consistently. Consider using ngAnnotate.

# Controller Patterns

## Role of a Controller

Controllers are effectively constructors, as they provide us with a new instance of the Controller. 

## Classic vs. New

## Why Dots Matter

* Dots provide clarity
* Easier to reach the parent scope with $parent.$parent chaining
* Remove unexpected behavior

## Nesting

Recommendation is to use Controller As syntax. Controller As syntax is essentially syntactic sugar to inject scope into the controller and provide encapsulation.

## $scope

Try to avoid using $scope.$watch and $scope.$broadcast in a controller.

    angular
      .module('app')
      .controller('Avengers', Avengers);
    
    Avengers.$inject = ['dataservice', 'logger'];

    function Avengers(dataservice, logger) {
      var vm = this;
      vm.avengers = [];
      vm.title = 'Avengers';

      activate();

      function activate() {
        return getAvengers().then(function() {
          logger.info('Activated Avengers View');
        });
      }

      function getAvengers() {
        return dataservice.getAvengers().then(function(data) {
          vm.avengers = data;
          return vm.avengers;
        });
      }
    }

## Resolvers

You can associate controllers to view using the `controller` and `controllerAs` property in a route config. Using the `resolve` property, to wire up a route resolver to perform activities you want to happen before you navigate to the route endpoint, such as prefetch data or create custom variables. You MUST use the `controller` and `controllerAs` properties in the route config when using `resolve`. Resolvers also work with promises. `resolveAlways` can be used to inject a resolver into all routes.

## Testing

When testing a controller using `controllerAs` syntax:

    beforeEach(function() {
      // ...
      scope = $rootScope.$new(); // scope is a variable on the describe
      controller = $controller('Avengers as vm', {
        '$scope': scope
      });
    });

## Controller Patterns

* Controller As provides syntactical sugar
* Dots help avoid pitfalls in bindings
* Use $scope members wisely (consider a factory)
* Capture 'this' to avoid context issues
  * `var vm = this`
* Define controller
  * in a view using ng-controller
  * in a route
* Resolve functions in the routes work with both
  * Resolve can be injected into Controller only when using ControllerAs

# Annotations and Code Analysis

## The Value of Task Automation

* Improved quality
* Deliver Faster
* Repeatable / More Consistent

Grunt - configuration over code (file based)
Gulp - code over configuration (stream based)

## ngAnnotate Tips

Use the `/* @ngInject */` hint to help ngAnnotate find locations to inject.

    // ngAnnotate won't be able to find functions that aren't referenced by Angular
    /* @ngInject */
    function foo1($scope) {}

    // or function expressions
    /* @ngInject */
    var foo2 = function($scope) {}

    // or object literals, like when we have a route resolver
    foo3 = /* @ngInject */ {
      controller: function($http) {},
      resolve: {
        data: function(thing) {}
      }
    }

# Exception Handling

Handling?

* Logging
* Reporting
* Diagnostics
* Notifying The User

## Catching Angular Errors Using Decorators

Decorating allows us to extend or override functionality in other services using `$delegate`

    (function () {
      'use strict';

      angular
        .module('blocks.exception')
        .provider('exceptionConfig', exceptionConfigProvider)
        .config(exceptionConfig);

      function exceptionConfigProvider() {
        this.config = {
          // appErrorPrefix: ''
        };

        this.$get = function () {
          return {
            config: this.config
          };
        };
      }

      exceptionConfig.$inject = ['$provide'];

      function exceptionConfig($provide) {
        $provide.decorator('$exceptionHandler', extendExceptionHandler);
      }

      extendExceptionHandler.$inject = ['$delegate', 'exceptionConfig', 'logger'];

      function extendExceptionHandler($delegate, exceptionConfig, logger) {
        var appErrorPrefix = exceptionConfig.config.appErrorPrefix || '';
        return function (exception, cause) {
          $delegate(exception, cause);
          var errorData = {exception: exception, cause: cause};
          var msg = appErrorPrefix + exception.message;
          logger.error(msg, errorData);
        };
      }
    });

## Handling Routing Exceptions

    function handleRoutingErrors() {
      // Route cancellation:
      // On routing error, go to the dashboard.
      // Provide an exit clause if it tries to do it twice.
      $rootScope.$on('$routeChangeError',
      function(event, current, previous, rejection) {
        if (handlingRouteChangeError) {
          return;
        }
        routeCounts.errors++;
        handlingRouteChangeError = true;
        var destination = (current && (current.title || current.name || current.loadedTemplateUrl)) ||
          'unknown target';
        var msg = 'Error routing to ' + destination + '. ' + (rejection.msg || '');
        logger.warning(msg, [current]);
        $location.path('/');
        }
      );
    }

## Handling Custom Exceptions with an Exception Catcher

    // in exception.js

    (function () {
      'use strict';

      angular
        .module('blocks.exception')
        .factory('exception', exception);

      /* @ngInject */
      function exception(logger) {
        var service = {
          catcher: catcher
        };
        return service;

        function catcher(message) {
          return function (reason) {
            logger.error(message, reason);
          };
        }
      }
    })();

    // in dataservice.js

    (function () {
      'use strict';

      angular
        .module('app.core')
        .factory('dataservice', dataservice);

      /* @ngInject */
      function dataservice($http, $location, $q, exception, logger) {
        var isPrimed = false;
        var primePromise;

        var service = {
          getAvengersCast: getAvengersCast,
          getAvengerCount: getAvengerCount,
          getAvengers: getAvengers,
          ready: ready
        };

        return service;

        function getAvengers() {
          return $http.get('/api/maa')
            .then(getAvengersComplete)
            .catch(function (message) {
              exception.catcher('XHR Failed for getAvengers')(message);
              $location.url('/');
            });

          function getAvengersComplete(data, status, headers, config) {
            return data.data[0].data.results;
          }
        }

        ...

## Summary

There's many ways an application can fail.

* Track and Correct
* Repeatable / Consistent Pattern
* Have an exception handling strategy
* Be consistent
* Track and log the exception patterns
* Graceful UX

# Using a Team Style Guide

Key Messages in a Style Guide

* What to follow
* Why your team chose it
* How to implement it

