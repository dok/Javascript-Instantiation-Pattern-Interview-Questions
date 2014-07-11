**Javascript Instantiation Pattern Interview Questions**

**What is the difference between pseudoclassical and prototypal instantiation?**
Pseudoclassical instantiation is another layer of abstraction over the prototypal instantiation. And it was a way for developers coming from Object oriented languages to be more familiar with the language.

Prototypal Pattern:
```javascript
function Shoe(color) {
	var instance = Object.create(Shoe.prototype);
	instance.color = color;
	return instance;
}

Shoe.prototype.wear = function() { ... };
```
Pseudoclassical Pattern:
```javascript
function Shoe(color) {
	// this = Object.create(Shoe.prototype); // This line will be injected into the method when using the ‘new’ keyword
  this.color = color;
	// return this; // This line will be injected into the method when using the ‘new’ keyword
}

var shoe = new Shoe(‘red’);
Shoe.prototype.wear = function() { ... };
```
**Implement an Animal class, Mammal subclass, Lion subclass. Use pseudoclassical instantiation.**
```javascript
var Animal = function(name) {
  this.name = name;
};

Animal.prototype.see = function() {
	console.log(this.name + " can see");
};

var Mammal = function(legs, name) {
  this.legs = legs;
  Animal.call(this, name);
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.walk = function() {
	console.log("I am walking with " + this.legs + " legs");
};

var Lion = function(weight, legs, name) {
	Mammal.call(this, legs, name);
	this.weight = weight;
};

Lion.prototype = Object.create(Mammal.prototype);
Lion.prototype.constructor = Lion;
Lion.prototype.roar = function() {
	console.log('RAWR!');
};
```
**Now do that using a prototypal instantiation pattern.**
```javascript
var Animal = function(name) {
	var instance = Object.create(Animal.prototype);
	instance.name = name;
	return instance;
};

Animal.prototype.see = function() {
	console.log(this.name + ' can see');
};

var Mammal = function(name, legs) {
	var instance = Animal(name);
	instance.legs = legs;
	return instance;
};

Mammal.prototype = Object.create(Animal.prototype);

var Lion = function(name, legs, weight) {
	var instance = Mammal(name, legs);
	instance.weight = weight;
	return instance;
};

Lion.prototype = Object.create(Mammal.prototype);
Lion.prototype.roar = function() {
	console.log(this.name + ' is roaring');
};

//usage

var lion = Lion('leon', 4, 400);
```
**What is the object ‘prototype’**
Is a javascript object with a very misleading name. It holds methods or properties to the function such that when it is used as a constructor, the instances that are derived from the maker function will have access to everything that is stored in the prototype object.

**What is ‘prototype.constructor’?**
Is a reference to the maker function that was used to create the particular instance.

**What does Object.create do?**
Object.create will establish a delegation relationship between the instantiation object and the object that has been passed.

For instance:
```javascript
function Shoe(color) {
	var instance = Object.create(Shoe.prototype);
	instance.color = color;
	return instance;
}

Shoe.prototype.wear = function() { ... };

var shoe = Shoe('blue');
```
when shoe.wear is called, it will try to find it in its own object. If it does not find it, it will bubble up to Shoe.prototype. And if it does not find it, it will bubble up to the Object prototype.

**Write a Vehicle class using a functional instantiation pattern and then a Car as a subclass.**
```javascript
var Vehicle = function(speed) {
	var instance = {};
	instance.speed = speed;
	instance.honk = function() {
		console.log('beep beep');
	};
	return instance;
};

var Car = function(type, speed) {
	var instance = Vehicle(speed);
	instance.type = type;
	instance.drive = function() {
		console.log('I am driving');
	};
	return instance;
};
```
**Now do that with a functional shared pattern**
```javascript
var honk = function() {
	console.log('Honking at ' + this.speed); 
};

var Vehicle = function(speed) {
	var instance = {};
	instance.speed = speed;
	instance.honk = honk;
	return instance;
};

var drive = function() {
	console.log('driving with a ' + this.type);
};

var Car = function(type, speed) {
	var instance = Vehicle(speed);
	instance.type = type;
	instance.drive = drive;
	return instance;
};
```
