# Javascript Design Pattern I
## Describe the concepts of Design Pattern

**Patterns are proven solutions:** They provide solid approaches to solving issues in software development using proven techniques that reflect the experience and insights the developers that helped define them bring to the pattern.

**Patterns can be easily reused:** A pattern usually reflects an out of the box solution that can be adapted to suit our own needs. This feature makes them quite robust.

**Patterns can be expressive:** When we look at a pattern there’s generally a set structure and vocabulary to the solution presented that can help express rather large solutions quite elegantly.

**Reusing patterns assists in preventing minor issues that can cause major problems in the application development process.** What this means is when code is built on proven patterns, we can afford to spend less time worrying about the structure of our code and more time focusing on the quality of our overall solution. This is because patterns can encourage us to code in a more structured and organized fashion avoiding the need to refactor it for cleanliness purposes in the future.

**Patterns can provide generalized solutions which are documented in a fashion that doesn't require them to be tied to a specific problem.** This generalized approach means that regardless of the application (and in many cases the programming language) we are working with, design patterns can be applied to improve the structure of our code.

**Certain patterns can actually decrease the overall file-size footprint of our code by avoiding repetition.** By encouraging developers to look more closely at their solutions for areas where instant reductions in repetition can be made, e.g. reducing the number of functions performing similar processes in favor of a single generalized function, the overall size of our codebase can be decreased. This is also known as making code more DRY.

**Patterns add to a developer's vocabulary, which makes communication faster.**

**Patterns that are frequently used can be improved over time by harnessing the collective experiences other developers using those patterns contribute back to the design pattern community.** In some cases this leads to the creation of entirely new design patterns whilst in others it can lead to the provision of improved guidelines on how specific patterns can be best used. This can ensure that pattern-based solutions continue to become more robust than ad-hoc solutions may be.

---
## Creational Patterns
Creational design patterns focus on handling object creation mechanisms where objects are created in a manner suitable for the situation we're working in. The basic approach to object creation might otherwise lead to added complexity in a project whilst these patterns aim to solve this problem by controlling the creation process.

Some of the patterns that fall under this category are: Constructor, Factory, Abstract, Prototype, Singleton and Builder.

### No Constructor / object literal
**Object literals** are often used to store and pass around isolated chunks of data, like configuration settings or the parameters to an AJAX request. They can also help reduce the number of globals in your code.
```javascript
const configuration = {
  id: `#some-div-container`,
  title: `Welcome to Disneyworld`,
  adultCount: 32,
  kidCount: 123,
  hasCoupon: true,
}
```
---
### Constructor Pattern
```javascript
// ES5
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;

  this.fullName = function() {
    return `${this.firstName} ${this.lastName}`;
  };
}

// ES6
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

// Use it like this:
var sam = new Person('Sam', 'Selikoff');
sam.firstName;    // "Sam"
sam.fullName();   // "Sam Selikoff"

sam.firstName = 'Samuel';
sam.fullName(); // Samuel Selikoff
```
**Hoisting**
An important difference between function declarations and class declarations is that function declarations are hoisted and class declarations are not. You first need to declare your class and then access it, otherwise code like the following will throw a ReferenceError:
```javascript
var p = new Person(); // ReferenceError

class Person {}
```
---
### Prototypes
**Prototypes** let you share public methods across objects. (They also let you use inheritance).
```javascript
// ES5
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Person.prototype.fullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

function Developer(firstName, lastName) {
  this.prototype = Object.create( Person );
  Person.call( this, firstName, lastName );
} 

Developer.prototype.fullName = function() {
  return `I am Mr.${this.lastName}, but you can call me ${this.firstName}`;
};

const person = new Developer( 'Jason', 'Bond' );
console.log( person.fullName() ); // returns 'I am Mr.Bond, but you can call me Jason'

// ES6
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

class Developer extends Person {
  constructor(firstName, lastName) {
    super(firstName, lastName);
  }

  fullName() {
    return `I am Mr.${this.lastName}, but you can call me ${this.firstName}`;
  }
}

const person = new Developer( 'Jason', 'Bond' );
console.log( person.fullName() ); // returns 'I am Mr.Bond, but you can call me Jason'
```
---
### Closures
**Closures** let you have private properties and methods. They are often used in libraries. (Closures are part of a larger pattern called the Module pattern).
```javascript
function Person(firstName, lastName) {
  var _firstName = firstName,
      _lastName = lastName;

  var my = {
    firstName: _firstName,
    lastName: _lastName
  };

  my.fullName = function() {
    return _firstName + ' ' + _lastName;
  };

  // Getter/setters
  my.firstName = function(value) {
    if (!arguments.length) return _firstName;
    _firstName = value;

    return my;
  };

  my.lastName = function(value) {
    if (!arguments.length) return _lastName;
    _lastName = value;

    return my;
  };

  return my;
}

// Use it like this:
var mark = Person('Mark', 'Twain'); // note: no `new` keyword!

mark.firstName('Samuel');
mark.lastName('Clemens');

mark.fullName(); // Samuel Clemens
```

---
## Structural Patterns
Structural patterns are concerned with object composition and typically identify simple ways to realize relationships between different objects. They help ensure that when one part of a system changes, the entire structure of the system doesn't need to do the same. They also assist in recasting parts of the system which don't fit a particular purpose into those that do.

Patterns that fall under this category include: Decorator, Facade, Flyweight, Adapter and Proxy.

### Facade Pattern
Facades pattern can often be seen in JavaScript libraries like jQuery where, although an implementation may support methods with a wide range of behaviors, only a "facade" or limited abstraction of these methods is presented to the public for use.
```javascript
var addEvent = function( elem, eventName, func ) {
  if( elem.addEventListener ) {
    elem.addEventListener( eventName, func, false );
  } else if( el.attachEvent ) {
    elem.attachEvent( "on" + eventName, func );
  } else{
    elem[ "on" + eventName ] = func;
  }
};
```



---
## Behavioral Patterns => (Streams Library)
Behavioral patterns focus on improving or streamlining the communication between disparate objects in a system.

Some behavioral patterns include: Iterator, Mediator, Observer and Visitor.

### Mediator
Huh?

---
### Observer Pattern
The Observer is a design pattern where an object (known as a subject) maintains a list of objects depending on it (observers), automatically notifying them of any changes to state.

When a subject needs to notify observers about something interesting happening, it broadcasts a notification to the observers (which can include specific data related to the topic of the notification).

When we no longer wish for a particular observer to be notified of changes by the subject they are registered with, the subject can remove them from the list of observers.

Another prime example is the model-view-controller (MVC) architecture; The view updates when the model changes. One benefit is decoupling the view from the model to reduce dependencies.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/1000px-Observer.svg.png)

*The snippet below is NOT an example on how to write an implementation of the observer pattern, as that can be quite long and verbose, just Google it. Rather, it is an example of how it is could be used with [RxJs](http://reactivex.io/) library as our implementation.* 
```javascript

class BootstrapModal {
  constructor( $dom ) {
    this.onOpen = new Rx.Subject();
    this.onClose = new Rx.Subject();

    this.$dom = $dom;
    $( this.$dom ).modal(); // initialize the modal
  }

  open() {
    $( this.$dom ).modal( 'show' );
    this.onOpen.onNext();
  }

  close() {
    $( this.$dom ).modal( 'hide' );
    this.onClose.onNext();
  }
}

const modal = new BootStrapModal( $( '#user-sign-up-modal' ) );

// Google Analytics tracking
modal.onOpen.subscribe( () => {
  ga.track( 'click', 'open user sign up modal' );
});

modal.onClose.subscribe( () => {
  ga.track( 'click', 'close user sign up modal' );
});

// Save user sign up data
modal.onClose.subscribe( () => {
  const firstName = $( modal.$dom.find( '#user-first-name') );
  const lastName = $( modal.$dom.find( '#user-last-name') );
  updateSignUpData( firstName, lastName );
});

```

---
References:
- http://www.samselikoff.com/blog/some-Javascript-constructor-patterns/
- https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes
- https://addyosmani.com/resources/essentialjsdesignpatterns/book/#whatisapattern
- 

