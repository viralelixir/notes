## Getting Started

### Concepts

- Model
- View
- Controller
  - Constructor function that creates a controller instance. Purpose is to expose variables and functionality to expressions and directives.
  - Can initialize in a template with the `ng-controller=""` directive inside an HTML tag.
  - Typically should use `ControllerName as controllerName` to save the controller to the variable `controllerName` in the current scope.
- Templates
- Directives
- Expressions
  - `{{ expression | filter }}` syntax, allows you to access the scope and evalutes to a value
  - A `filter` formats the value of an expression for display to the user
- Dependency Injection


### Tutorials

Following along with [this tutorial](https://egghead.io/series/angularjs-app-from-scratch-getting-started).

Should probably be using John Papa's [Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md).

### Controllers

One way communciation:

```js
// Controller logic
angular.module('myApp', [])
  .controller('MainCtrl', function ($scope) {
    $scope.categories = [{ id: 0, name: 'Development' }];
  });
```

```html
<!-- Template -->
<body ng-controller="MainCtrl">
  <li ng-repeat="category in categories">
    <a href="#">{{category.name}}</a>
  </li>
</body>
```

Two-way communication:

```js
// Controller logic
angular.module('myApp', [])
  .controller('MainCtrl', function ($scope) {
    $scope.categories = [{ id: 0, name: 'Development' }];
    $scope.setCurrentCategory = (category) => scope.currentCategory = category;
  });
```

```html
<!-- Template -->
<body ng-controller="MainCtrl">
  <li ng-repeat="category in categories">
    <a href="#" ng-click="setCurrentCategory(category)">{{category.name}}</a>
  </li>
</body>
```

## Managing Simple States

This is a simple example, but probably shouldn't be setting scope directly!

```js
angular.module('myApp', [])
  .controller('MainCtrl', function ($scope) {
    $scope.categories = [{ id: 0, name: 'Development' }];
    $scope.setCurrentCategory = (category) => scope.currentCategory = category;
    
    $scope.isCreating = false;
    $scope.isEditing = false;
    
    $scope.startCreating = function startCreating() {
      $scope.isCreating = true;
      $scope.isEditing= false;
    };
    
    $scope.cancelCreating = function cancelCreating() {
      $scope.isCreating = false;
    };
    
    $scope.startEditing = function startEditing() {
      $scope.isCreating = false;
      $scope.isEditing = true;
    };
    
    $scope.cancelEditing = function cancelEditing() {
      $scope.isEditing = false;
    };
    
    $scope.shouldShowCreating = () => $scope.currentCategory && !$scope.isEditing;
    $scope.shouldShowEditing = () => $scope.isEditing && !$scope.isCreating;
  });
```

```html
<!-- Template -->
<body ng-controller="MainCtrl">
  <div ng-if="shouldShowCreating()">
    <button class="close">&times;</button>
    <button ng-click="startEditing()"></button>
  </div>
  <div ng-if="shouldShowEditing()">
    <button class="close">&times;</button>
    <button ng-click="startCreating()"></button>    
  </div>
</body>
```
