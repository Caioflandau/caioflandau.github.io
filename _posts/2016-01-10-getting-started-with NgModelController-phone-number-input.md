---
layout: post
title:  "AngularJS: Getting Started With NgModelController - Creating a Phone Number Input Component"
date:   2016-01-10 19:00:00 -0200
categories: angularjs
---
There seems to be a lot of confusion in understanding how to setup custom input elements using NgModelController. I know I struggled a bit, so here's my attempt to help.

## The concept in two simple steps
When creating a component that will use NgModelController for two-way data binding, we need to remember exactly that: it's **two-ways**.
Here's what that means. We need to care, in very simple terms, about data moving in two directions: _from_ our component and _to_ our component:

### 1. Data _from_ our component to the exposed model: `$setViewValue()`
When data changes _inside_ our component (usually due to user interaction with it), we need to tell Angular what happened.
That's where `NgModelController.$setViewValue()` comes into play. We'll see how in a sec. For now, let's see how data moves the other way around.

### 2. Data _to_ our component from the exposed model: `$render()`
What about when data is changed from outside of our component's scope? Say something changed the value of the model. We'll want to reflect those changes on our component.
You know, when an `<input>` text gets updated when you change their ng-model value?
For that, Angular will notify our component by calling `NgModelController.$render()`. We'll write that function ourselves!

Let's see how all that is done. It's simpler than it sounds:

## The example
We'll work on a simple international phone number input directive, composed of a country code picker and a text field for the number.

### Premises:

1. We want to be able to use it as `<phone-number-input ng-model="someModel"></phone-number-input>`
1. We'll want the model to be an object like `{ countryCode: "US", number: "999-999-9999" }`
1. Any changes to the model should be reflected in the component's view

Here's the initial code for that directive:

#### Template
{% highlight html %}
<!-- phone-number-input.html -->
<div class="phone-input">
  <label>Phone number</label>
  <select ng-model="data.countryCode" ng-options="country.value as country.label for country in vm.countries"></select>
  <input ng-model="data.number" placeholder="Number (123-456-7890)" />
</div>
{% endhighlight %}

#### Javascript
{% highlight javascript %}
// phone-number-input.js
var app = angular.module('phoneNumberInput', []);

app.directive('phoneNumberInput', function() {

  function link(scope, element) {
    // These are the models for the country code select and the number input:
    scope.data = {
      countryCode: undefined,
      number: undefined
    };
  }

  function PhoneNumberCtrl() {
    // Exposed as vm.countries:
    this.countries = [{
      value: 'US',
      label: 'US (+1)'
    }, {
      value: 'UK',,
      label: 'UK (+44)'
    }, {
      value: 'BR',,
      label: 'BR (+55)'
    }];
    //Note: There are countries with the same code (notably US and CA). That's why we're using the actual country name as the identifier!
  }

  return {
    restrict: 'E',
    templateUrl: 'phone-number-input.html',
    scope:{},
    link: link,
    controller: PhoneNumberCtrl,
    controllerAs: 'vm'
  }
});
{% endhighlight %}

### Exposing the model through NgModelController
Now here's the interesting part. We actually don't need that many changes on that code to make it work with NgModelController!

First thing we need to do is require `ngModel` on our directive. Add the following to our directive's code:
{% highlight javascript %}
// phone-number-input.js
app.directive('phoneNumberInput', function() {

  // Noticed how we've added a 4th parameter, called ngModelController? Angular will pass it in as our `NgModelController` instance
  function link(scope, element, attrs, ngModelController) {
    //...
  }
//...
  return {
    restrict: 'E',
    templateUrl: 'phone-number-input.html',
    require: 'ngModel', // <- Requires the NgModelController from the ngModel directive
//...
{% endhighlight %}

Remember the two ways data can move? Let's implement the first one:

#### Syncing data _from_ our component to the exposed model:
When the user changes something (selects a country code or types something in the number input), we'll need to tell Angular what happened, and how the new model looks like.

Here's how that's done. We'll start by adding `ng-change` directives on the inputs. That way our directive is notified when the user types something or selects a country:
{% highlight html %}
<!-- phone-number-input.html -->
<!-- ... -->
<select ng-change="onDataChanged(data)" ng-model="data.countryCode" ng-options="country.value as country.label for country in vm.countries"></select>
<input ng-change="onDataChanged(data)" ng-model="data.number" placeholder="Number (123-456-7890)" />
<!-- ... -->
{% endhighlight %}

And then add the `onDataChanged` function in the directive's scope:
{% highlight javascript %}
// phone-number-input.js
function link(scope, element, attrs, ngModelController) {
  //...

  //Called when the user selects a country code or changes the number input:
  scope.onDataChanged = function(newData) {
    // This is our ng-model object that gets exposed, fulfilling premise #2:
    var newModelValue = {
      countryCode: newData.countryCode,
      number: newData.number
    };
    // This is how we notify Angular that data changed, passing ngModelController the new model value:
    ngModelController.$setViewValue(newModelValue);
  }
  //...
}
{% endhighlight %}
And that's it for the first data direction! All we needed to do was call `ngModelController.$setViewValue(newModelValue)` when data changed due to user interaction with the component.

Now for the other way data moves:

#### Syncing data _to_ our component from external changes

Now that our component updates the external model when the user changes something, we'll need to also handle the other way around. That is, *how do I sync data **back** to the component's view* when something outside the component's scope changes the model's value.

In other words, say I wanted to select a country code from a controller that uses our component. I should be able to set `model.countryCode = 'US'` from that controller, and that would select US in our picker, right?

Here's how to use the `$render` function for that:

{% highlight javascript %}
function link(scope, element, attrs, controllers) {

  var ngModelController = controllers[1];

  // These are the models for the country code select and the number input:
  scope.data = {
    countryCode: undefined,
    number: undefined
  };

  //...

  // Remember we were going to implement the $render() function ourselves? This is it.
  // We set the $render function in ngModelController, and Angular will call it when something changes our model:
  ngModelController.$render = function() {
    // Someone changed our model! Let's update our view to reflect those changes:
    var changedModel = ngModelController.$viewValue;

    // The code here varies a lot based on what component you're syncing data to. It could be a third-party component that needs you to call a setter function to update data, for example.
    // In our case, setting scope.data is what it takes to update the views, since they are basic inputs using their own ng-models:
    scope.data = {
      countryCode: changedModel.countryCode,
      number: changedModel.number
    };
  }
}
{% endhighlight %}

And that's all there is to it! We have a fully-working custom ngModel implementation.

### Full code / usage example

The full code for the phone number component, along with an usage example, can be found [here](http://plnkr.co/edit/vYC4p5)

### Further reading

This is just the bare minimum implementation. There are many things you can/should do to make the implementation bulletproof.

For example, we should never assume the object in `$viewValue` when `$render` is called is in the format you expect it to be.
The data there comes from something not under the component's control, so without a `$parsers` pipeline in place, it could be in _any_ crazy format. Always do sanity-checks.

You might find it useful to setup a few other things, which are outside of this post's scope:

* The `$parsers` array, as mentioned, is used to sanitize data from the `$viewValue` before it gets committed to the model.
* The `$formatters` array would be used to convert the model value into a format that makes sense for your view to render it.
* Many more possibilities!
