# Angular Day

## Setup

Make sure you have [Node
installed](http://nodejs.org/download/), then open up a terimnal and
type the following commands:  
```shell
git clone https://github.com/CodeCoreYVR/angular_day && cd angular_day
gem install compass
npm install -g bower
npm install -g grunt-cli
npm install
bower install
grunt install
grunt serve
```  

[angular docs](https://docs.angularjs.org/api) |
[promises](https://egghead.io/lessons/angularjs-promises) | [angular
sprinkles](https://github.com/BrewhouseTeam/angular_sprinkles)   
Angular started in 2009, was originally a Google project. Google made it
an official project, because he was able to rewrite one of Google's
projects from 17,000 lines of code, to 2000.  
  
Angular is a framework for your JavaScript, just like Ruby on Rails is a
framework for writing a Ruby HTTP Server. Angular is sort of the same
thing, but for the front end for your JavaScript. It follows
conventions, such as in Rails: If you want to have a controller, it has
to be in your controllers folder. Additionally, the filename has to be
named the same thing as the classname.  
  
Angular is not as strict as Rails. There are still debates on which way
is better to do things. Do you clump each thing for one feature in one
folder, or do you spread them out, like rails, where you would have a
styles directory, etc.  
  
Angular forces a paradigm that is extremeley removed from the jQuery
framework of JavaScript that maybe you're used to writing. In jQuery,
you're probably used to finding something by its claess. You wan
something to find something when you click it. Put a class on it, then
call it with a `.click` for example.
```jquery
$('.some-selector').click(); // jQuery
// ^ don't even think about it!
```
  
Angular is much different from that. There are some other frameworks out
there, such as Polymer, Brick. They don't completely take control of
your JavaScript framework, as Angular does.  
  
The first thing we need to do is configure out project so that it is an
angular project. First, we are going to go to the `<html>` element and
add the attribute `ng-app="app"`.
```html
<!-- app/index.html -->
<!doctype html>
<html class="no-js" ng-app="app">
  <head>
    <meta charset="utf-8">
    <title></title>
```
Then go to `app/scripts/app.js`
```javascript
// app/scripts/app.js
angular.module('app', []);
```
Let's go back to the `index.html` to our container class  
```html
<!-- app/index.html -->

  <div class="container">
    Hello World!
  </div>
```
And let's add an input type to it. Note also that Angular uses the
double curly braces style for interpolation `{{}}`  
```html
<!-- app/index.html -->

  <div class="container">
    <p>{{ gabe }}</p>
    <input type="text" ng-model="gabe">
  </div>
```
Angular has a way to predefine some functionality of a DOM element. It
does this with something called a `controller`. You can probably guess
what attribute we give an element of this type: `ng-controller`.  
  
If we just give our container class an ng-controller attribute, we will
get an error stating that we have not defined a controller.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <p>{{ gabe }}</p>
    <input type="text" ng-model="gabe">
  </div>
```
To overcome this error, we will open up our `app/scripts/app.js`  
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function() {

});
```
When you create a new controller, it creates a scope. The scope holds
the state or the data within the element. In this case, the controller
`foo` has its scope set within the div `class="container"`.   
  
Angular has a really fancy way of adding things (sort of like saying
`require` in ruby). We can put `$scope` in Angular.
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.gabe = "bar"
});
```
Let's add another controller within our other controller in the
index.html
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <p>{{ gabe }}</p>

    <input type="text" ng-model="gabe">

    <div ng-controller="bar">
      <input type="text" ng-model="gabe">
    </div>

  </div>
```
Add the controller to the javascript
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.gabe = 'bar';
});

app.controller('bar', function($scope) {
  $scope.gabe = 'potato';
}
```
It is convention (and this is important, you can avoid a lot of problems
by following it) to always use dot notation. The way you do that is you
never assign just a string to a variable within a scope, you assign an
object `{}` with a key and a value.
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.gabe = {
    bar: 'bar'
  };
});

app.controller('bar', function($scope) {
  $scope.gabe = {
    potato: 'potato'
  };
}
```
Which means we need to reference the variable with dot notation in the
index.html
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <p>{{ gabe.bar }}</p>

    <input type="text" ng-model="gabe.bar">

    <div ng-controller="bar">
      <input type="text" ng-model="gabe.bar">
      <input type="text" ng-model="potato">
    </div>

  </div>
```
When people talk about Angular, they usually talk about 2 things: 2 way
data binding, and directives. Directives are a way of creating custom
HTML components. This is actually a really fantastic feature, as we will
see. `ng-model`, and `ng-controller` are examples of directives.  
  
Let's go back to the app.js and delete some of the controllers.  
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {

});
```
The first thing we are going to talk about is `ng-repeat`. Which acts
similar to how `array.each do |item|` works in ruby  
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.data = {
    pets: ['cat', 'dog', 'fish', 'bird', 'potato', 'rock']
  };
});
```
Now back to our view `index.html`
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets">
      <p>Hello, {{ pet }}!</p>
    </div>
  </div>
```
What if I want to go through my list of pets, but I only want to show
them if they are not a cat. Angular has a built in directive called
`ng-if`  
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets">
      <p ng-if="pet != 'cat'">Hello, {{ pet }}!</p>
    </div>
  </div>
```
Something similar to `ng-if` is `ng-show`. This acts similar however, it
hides the element rather than not even add it to the DOM, as `ng-if`
would do. There is another ng directive `ng-hide` which would in effect
do the opposite.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets">
      <p ng-hide="pet === 'cat'">Hello, {{ pet }}!</p>
    </div>
  </div>
```
More common directives, let's have a look at `ng-click` with a button
element
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets">
      <button ng-click="showAnimal(pet)">Hello, {{ pet }}!</p>
    </div>
  </div>
```
Now, let's gop back to the javaScript and define a function `showAnimal`
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.data = {
    pets: ['cat', 'dog', 'fish', 'bird', 'potato', 'rock']
  };

  $scope.showAnimal = function(animal) {
    alert(animal);
  };
});
```
Let's add a counter to our view and controller:
view
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    {{ data.counter }}
    <button ng-click="counter = counter + 1">click me</button>
    <div ng-show="counter > 5">Hello!</div>
  </div>
```
controller
```javascript
// app/scripts/app.js
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.data = {
    pets: ['cat', 'dog', 'fish', 'bird', 'potato', 'rock']
  };

  $scope.counter = 0;

  $scope.showAnimal = function(animal) {
    alert(animal);
  };
});
```
And, let's have a look at `ng-class`. I pass in an object, and the keys
are the classes that I want to add to this element. So let's say that
this div, I want to add a class `foo` if the counter is greater than 5.
So, this div is going to have the class `foo` if the counter is greater
than 5, and the class `bar` if greater than 10
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    {{ data.counter }}
    <button ng-click="counter = counter + 1">click me</button>
    <div ng-class="{foo: (counter > 5), bar: (counter >
10)}">Hello!</div>
  </div>
```
Add a quick style block to see the difference
```html
<!-- app/index.html -->
  <style>
    .foo {
      color: #00f;
    }
    .bar {
      font-size: 48px;
    }
  </style>

  <div class="container" ng-controller="foo">
    {{ data.counter }}
    <button ng-click="counter = counter + 1">click me</button>
    <div ng-class="{foo: (counter > 5), bar: (counter >
10)}">Hello!</div>
  </div>
```
## Built in Services
So, we just finished talking about some of Angular's built-in
directives. We're going to talk about built-in Services.  
  
We are now going to include resources by adding `ngResource` to the
second argument in `angular.module` in our app.js. This will give us
access to `$resource`. "Resource" is a service that allows you to
connect to a RESTful API, and download some content from it. Make sure
to add `.json` to the end of the url, so we get json returned.  
```javascript
// app/scripts/app.js
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  var resource =
    $resource('http://fundsy.herokuapp.com/campaings/:id.json', { id:
'@id' })
  
  debugger // put a debugger in your controller: javaScript will load
until it gets to this point, then it will pause for you

  $scope.campaigns = resource.query();
});
```
Now, let's go back to the view and add a div using `ng-repeat`
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="campaign in campaigns">
      {{ campaign }}
    </div>
  </div>
```
These objects come back with `title`, `description` and some [other
fields](http://fundsy.herokuapp.com/campaigns.json). Let's pretty this
up.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="campaign in campaigns">
      </h1>{{ campaign.id }}: {{ campaign.title }}</h1>
      <p>{{ campaign.description }}</p>
      <p>{{ campaign.end_date }}</p>
    </div>
  </div>
```
Let's take a look at some of the other things that resources are capable
of doing. We will extend the paragraph, adding a button called
'ng-click' and we'll give it a function called `showCampaign` and pass
in the campaign as an argument.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="campaign in campaigns">
      </h1>{{ campaign.id }}: {{ campaign.title }}</h1>
      <p>{{ campaign.description }}</p>
      <p>
        Ends on: {{ campaign.end_date }}
        <button ng-click="showCampaign(campaign)">show</button>
      </p>
    </div>
  </div>
```
Now, we can add a function `showCampaign` to the scope of our app
controller
```javascript
// app/scripts/app.js
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  var resource =
    $resource('http://fundsy.herokuapp.com/campaings/:id.json', { id:
'@id' })
  
  debugger // put a debugger in your controller: javaScript will load
until it gets to this point, then it will pause for you

  $scope.campaigns = resource.query();

  $scope.showCampaign = function(campaign) {
    //alert(campaign.title);
    alert(campaign.id);
  };
});
```
Add a current campaign and set it to null
```javascript
// app/scripts/app.js
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  var resource =
    $resource('http://fundsy.herokuapp.com/campaings/:id.json', { id:
'@id' })
  
  debugger // put a debugger in your controller: javaScript will load
until it gets to this point, then it will pause for you
  
  $scope.currentCampaign = null;
  $scope.campaigns = resource.query();

  $scope.showCampaign = function(campaign) {
    //alert(campaign.title);
    alert(campaign.id);
  };
});
```

In the view, make a new block with a new element and use
`ng-show="currentCampaign"` so it will only show this element if there
is a current campaign set.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-show="currentCampaign" style="border: 1px soliud black;">
      <h1>Current Campaign</h1>
      <p>{{ currentCampaign.title }}</p>
    </div>

    <div ng-repeat="campaign in campaigns">
      </h1>{{ campaign.id }}: {{ campaign.title }}</h1>
      <p>{{ campaign.description }}</p>
      <p>
        Ends on: {{ campaign.end_date }}
        <button ng-click="showCampaign(campaign)">show</button>
      </p>
    </div>
  </div>
```
***1.) When you click the "show" button, make the current campaign
appear at the top (that block we had that is currently invisible).***
```javascript
// app/scripts/app.js
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  var resource =
    $resource('http://fundsy.herokuapp.com/campaings/:id.json', { id:
'@id' })
  
  debugger // put a debugger in your controller: javaScript will load
until it gets to this point, then it will pause for you
  
  $scope.currentCampaign = null;  // at first our campaign is set to
null
  $scope.campaigns = resource.query();

  $scope.showCampaign = function(campaign) {
    // add this scope, to pass in the campaign we click
    $scope.currentCampaign = campaign
  };
});
```
If you wanted to maybe save the campaign when you clicked on show, you
could add something like `campaign.save(); within the `showCampaign`
method.  
  
Let's clear everything out and start with a fresh slate again. Then add
a get request for a campaign, using the [$http
service](https://docs.angularjs.org/api/ng/service/$http), rather than
the resource service.  
```javascript
// app/scripts/app.js
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  baseUrl = 'http://fundsy.herokuapp.com/campaigns'
  //var resource =
  //  $resource(baseUrl + '/:id.json', { id: '@id' });
  
  // $scope.campaign = resource.get({ id: 3 });

  // Get a specific campaign
  $http.get(baseUrl + '/3.json').then(function(response) {
    $scope.campaign = response.data;
  });

  // Get a list of campaigns
  $http.get(baseUrl + '.json').then(function(response) {
    $scope.campaigns = response.data;
    debugger
  });
});
```
html
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <h1>{{ campaign.title }}</h1>
    <p>{{ campaign.id }}</p>
  </div>
```
Let's talk about 'Promises' in JavaScript and Angular. This is kind of
an advanced topic, and people who even use Angular in their daily lives
as people, don't fully understand. There is a built-in feature to
Angular that is important. $http uses that.  
  
When we say `$http.get`. When we used `$resource`, we could do
`$scope.comaing = $http.get(baseUrl + '/3.json');`. The best analogy to
a JavaScript promise is a `cheque`. You don't actually have the money
until you cash in that cheque. The cheque doesn't necessarily represent
money; it represents a 'promise of money'.  
  
`$http.get` returns a promise, and then when the promise is done
resolving, we need to do something with the response. In the following
example, the `response` argument represents the money.
```javascript
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  baseUrl = 'http://fundsy.herokuapp.com/campaigns'

  var promise = $http.get(baseUrl + '/3.json');

  promise.then(function(response) {
    $scope.campaign = response.data;
    debugger
  });

  $scope.campaign = { id: 0, title: 'Potato' };
  debugger
});
```
What's the difference between a promise and a callback?
```javascript
// jquery callback
$.ajax({ method: 'GET', url: baseUrl }, function() {
  $scope.campaign = response.data;
});
```
The callback is being passed in as an argument. If you have to have a
callback within a callback, then you have to nest the callbacks inside
of each other. You don't have to do that with promises. Promises are
better.  
  
## Filters
[docs](https://docs.angularjs.org/guide/filter)  
```javascript
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  $scope.data = {
    pets: ['cat', 'dog', 'monkey', 'bird', 'fish']
  }
});
```
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets | limitTo:2">
      <p>Hello {{ pet }}!</p>
    </div>
  </div>
```
We can create our own filters, too! Let's create `noCatsAllowed`
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div ng-repeat="pet in data.pets | noCatsAllowed">
      <p>Hello {{ pet }}!</p>
    </div>
  </div>
```
Create the actual filter in our `app.js`
```javascript
angular.module('app', [ngResource]);

app.controller('foo', function($scope, $resource) {
  $scope.data = {
    pets: ['cat', 'dog', 'monkey', 'bird', 'fish']
  };
});

app.filter('noCatsAllowed', function() {
  return function(pets) {
    var friends = [];
    pets.forEach(function(pet) {
      if (pet != 'cat') {
        friends.push(pet);
      }
    });
  };
});

```
## Services
[service docs](https://docs.angularjs.org/api/ng/service) | [Service
versus
Factory](http://stackoverflow.com/questions/14324451/angular-service-vs-angular-factory)  
Let's start with writing our own services. Open your js file again.
We'll pass in `petCapitalizer` into our controller, then create an
`app.service`.
```javascript
angular.module('app', []);

app.controller('foo', function($scope, petCapitalizer) {
  var pets = ['cat', 'dog', 'monkey', 'bird', 'fish'];
  
  $scope.data = {};
  $scope.data.pets = $scope.data.pets.map(function(pet) {
    return petCapitalizer(pet);
  });
});

app.service('petCapitalizer', function() {
  return function(pet) {
    // we're going to take a string, like 'cat', and return 'Cat'
      return pet[0].toUpperCase() + pet.slice(1) + '!';
  };
});
```
In addition to `service` there is another keyword `factory`, which acts
quite similarly.
## Custom Directives
[directives doc](https://docs.angularjs.org/guide/directive)

```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
});

app.directive('fooBar', function() {
  return {
    restrict: 'EAC',
    template: 'HELLO!'
  };  
});
```
Notice how in the JavaScript we use `fooBar`, but in the HTML, we use
`foo-bar`.
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <foo-bar></foo-bar>
    <div foo-bar></div>
    <div class="foo-bar"></div>
  </div>
```
`link` is an argument that directives take, and it is loaded when the
directive gets loaded. Link is a key that has a function as its value.
Let's take a look.  
  
Interval accepts 2 arguments, a function, and the amount of time in
between running those functions. Let's add that, too!
```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
});

app.directive('fooBar', function($interval) {
  return {
    restrict: 'EAC',
    template: 'HELLO!',
    link: function(scope, element, attrs) {
      var opacity = 1;
      
      $interval(function() {
        element.css({ opacity: opaticy });
        opacity = -(opacity - 1);
      }, 1000);
    }
  };  
});
```
Why don't we change the name, because 'foo-bar' doesn't really explain
what is going on. We'll change it to blink.
```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
});

app.directive('blink', function($interval) {
  return {
    restrict: 'EAC',
    template: 'HELLO!',
    link: function(scope, element, attrs) {
      var opacity = 1;
      
      $interval(function() {
        element.css({ opacity: opaticy });
        opacity = -(opacity - 1);
      }, 1000);
    }
  };  
});
```
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div data-blink></div>
  </div>
```

Let's add a 'scope' to the directive. Directives are sort of like
functions, so they can take arguments. We'll add '$interval', and in the
scope we'll add interval' . Scope variables can be set to the following:
'=','@', '&'. '=' evaluates the variable being called, '@' doesn't try
to evaluate, it returns literally what it is set to (the variable name).
```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.time = 100;
});

app.directive('blink', function($interval) {
  return {
    restrict: 'EAC',
    scope: {
      interval: '=' // '=', '@', '&'
    },
    template: 'HELLO!',
    link: function(scope, element, attrs) {
      var opacity = 1;
      
      $interval(function() {
        element.css({ opacity: opaticy });
        opacity = -(opacity - 1);
      }, scope.interval);
    }
  };  
});
```
Now, we change the view to use the interval
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <div data-blink interval='time'></div>
  </div>
```
Let's bind the value of the number in the input on the view to the time
interval for our blink function!  
  
Set up the HTML
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <input type="number"  ng-model="time">
    <div data-blink interval='time'></div>
  </div>
```  
  
And set the javascript
```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.time = 100;
});

app.directive('blink', function($interval) {
  return {
    restrict: 'EAC',
    scope: {
      interval: '=' // '=', '@', '&'
    },
    template: 'HELLO!',
    link: function(scope, element, attrs) {
      var opacity = 1;

      var timeout = function() {
        $timeout(function() {
        element.css({ opacity: opaticy });
        opacity = -(opacity - 1);
        timeout();
        }, $scope.interval);
      };
      timeout();
    }
  };
  
});
```
Let's set another attribute in the scope called `content`.  
```javascript
angular.module('app', []);

app.controller('foo', function($scope) {
  $scope.time = 100;
  $scope.content = 'hello!';
});

app.directive('blink', function($interval) {
  return {
    restrict: 'EAC',
    scope: {
      interval: '=', // '=', '@', '&'
      content: '='
    },
    template: {{ content }},
    link: function(scope, element, attrs) {
      var opacity = 1;

      var timeout = function() {
        $timeout(function() {
        element.css({ opacity: opaticy });
        opacity = -(opacity - 1);
        timeout();
        }, $scope.interval);
      };
      timeout();
    }
  };
  
});
```
And in the HTML, we can do this
```html
<!-- app/index.html -->

  <div class="container" ng-controller="foo">
    <input type="number"  ng-model="time">
    <input type="text"  ng-model="content">
    <div data-blink interval='time'></div>
  </div>
```
