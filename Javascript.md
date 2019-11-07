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
Declara with constuctor
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
