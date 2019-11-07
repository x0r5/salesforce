# Javascript

## Objects
`Object.create()`

Object Literal Notation
```js
const complex = {
    real: 2,
    imag: 1,
    abs: function(){
        return sqrt(this.real*this.real + this.img*this.img);
    }
}
```
Declare with constuctor
```js
function Bike(gears, startGear){
    this.gear = gears;
    this.startGear = startGear
}

Bike.prototype.changeGear = function(direction, changeBy){
    this.currentGear += changeBy;
}
const Bike = new Bike(10, 3);
bike["gear"]; == bike.gear;
```

## Functions and Events
Declaration:
```js
function calculateGearRatio(drivenGear, driverGear){
    return (drivergear / drivenGear);
}
// call funciton
let gearRatio = calculateGearRatio(5, 8);

//same declaration:
const calculateGearRation = function(driverGear, drivenGear){...}
```
Anonymous Functions
```js
let myArray = [1, 4, 6, 7];
let newArray = myArray.map( function(item){return item / 2});
//map: takes a functon that is executed on every item
```

Assign Function Variable
```js
let result = bike.calculate();
const calculatefuncton = bike.calculate();
//call it
calculatefunction(); //called on bike obj just like calling bike.calculate();
```

Event Handling
```js
car handleClick = function(event){
    console.log(event.type); //click
    console.log(event.currentTarget); // the thing that is clicked
    console.log(event.screenX); // X coordinate
    console.log(event.screenY);
}
//html:
<button onclick="handleClick(event)"></button>
```
Same with Event Listener
```js
let button = document.getElementById("clicker");
button.addEventListener("click", handleClick);

//or
button.removeEventListener("click", handleClick);

//same with annymous function -> can`t be removed..
button.addEventListener("click", function(event{...}));
```


Scope
- variable scope
    - `var`: the scope is the whole function! not just the block
    - `let`
    - `const`
```js
function countToThree(){
    // i is in the scope of the countToThree function
    for( var i = 0; i < 3; i++){
        console.log(i);
    }
    console.log(i); //logs 3
}
```
Context and the This Keyword
> the `this` keyword references to the current ceontext<br>
> which is the container the function was called in<br>
> basically it is the `window` object