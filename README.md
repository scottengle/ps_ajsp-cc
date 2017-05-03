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
