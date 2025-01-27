# TypeScript-Getting-Started

This is the official repository for my Pluralsight course titled [*TypeScript: Getting Started*](https://app.pluralsight.com/library/courses/typescript-getting-started/table-of-contents). 
The *main* branch contains code as it 
exists at the start of the course. There are separate branches named after the modules in the course that contain the code as it 
exists at the end of that module.

# Quick Developer Overview

## Getting Started

* Write any file in ts compile it to JS file using command ```tsc filename.ts```
* Refer to the .js file from the html source code using tag ```<script src="FileName.js"></script>```
* Turn the **strict** mode on by adding the following line to ensure all validations are run while saving changes:

```json
{
  "compilerOptions": {
    "strict": true, 
    "watch": true
  }
}
```
* To compile a TS file use the following command on terminal window

```unix
tsc filename.ts secondfile.ts thirdfile.ts --outDir "../js"
```

* JS elements may usually throw error as **Object is possibly 'null'** when compiling ts file to js. This can be removed by adding not null assertion as shown below

```ts
// Before null assertion
messageElement.innerText = 'Welcome to the Page !';
document.getElementById('buttonName').addEventListener('click', functionName);

//After null assertion
messageElement!.innerText = 'Welcome to the Page !';
document.getElementById('buttonName')!.addEventListener('click', functionName);
```

* Note that **Chrome Developer Tool**(F12) is a great way to debug typescript issues. In Dev tools open Source > Pages. This should show the folder structure, putting a break point on the ts file code should help in debugging. Remember that the page is rendering from js file, but it knows that the source is the ts file



### tsconfig.json
* This file has all the options to start a typescript project. Once tsconfig.json is defined then only typing ```tsc``` in the terminal window should start the project instead of compiling all ts files individually and typing npm start 
* For compiler options visit TypeScript doc site - [Link](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
* Start a **Default config json** by using command ```tsc --init```
* add "files" array after the "compileOptions" entries in tsconfig. This is to indicate what files should be compiled  example:

```json
"files":[
  "app/app.ts"
]
```

* adding files to compile can also be done by using glob. Instead of the above format, use the following to pick up anyfile that matches the pattern

```json
"include":[
  "./**/*"
]
```
* Multiple tsconfig files can be used to take effect in hierarchy. Consider creating base version at root level with base value that is needed everywhere. In the inheriting config file use the following tag to refer to the base value

```json
{
  "extends": "../tsconfig.base",
  "compilerOptions":{
    "strict": "true"
  }
}
```
* ```outFile``` is a compiler option attribute that allows all ts files to be compiled and converted to one js file. Example below.
* Note if using module bundler then do not use ```outFile``` option

```ts
"compilerOptions":{
    "outFile": "../js/app.js"
  }
```

* To use module make following changes to ts config to specify what module compiler or bundle option to use 

```ts
"compilerOptions":{
    "module": "commonjs"
  }
```


### Webpack
* Webpack configuration allows npm to understand dependencies and source and target of compilation
Example **webpack.config.js**

```javascript
module.exports = {
  entry: './app/app.ts',
  devtool: 'inline-source-map',
  mode: 'development',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [ '.tsx', '.ts', '.js' ]
  },
  output: {
    filename: 'bundle.js'
  },
  devServer: {
    inline: false
  }
};

```

## Built-in Types
* Built-in TS types
* ```var``` vs. ```let``` , ```const```
* Type annotations and type interface
* Manage null and undefined
* Controlflow-based type analysis


**Built-in TS Types**
* Boolean, Number, String (Single quote, double quote, tick quote) , Array (primary data structure) are usual types same as java script
* Enum are special case in typescript
* Consider declaring variables before using it, though native js types such as ```var``` is allowed to be declared later. Recommended type instead of ```var``` is ```let``` or ```const```
* Difference between let and const is that variable with let can be reassigned with a value, const does not allow to change



## Annotations
* Annotations are used to declare type of variable. Though it is not mandatory, compiler forces type by default during initialization. Annotation makes it explicit. Example:

```ts
let x:string = "Example of a String !"
```
* Types can also be of union type to allow multiple type of values. Example:

```ts
let x: string | number ;
```

**Annotations in Function**
* JS considers all parameters to function as optional, allows more than declared number of parameters as well as any type. In TS it is considered required and recommended to declare type of parameter. Checking the number of parameters and type of parameter helps debug problems during development instead of catching them later in production. To make a parameter optional use '?'. Example:

```ts
function functionName(param_name: number, param_name?: string): string {
  //function body here
}
```

* Inheriting a property from JS, TS allows variable by default as type - any. However, this can be suppressed by having **compilerOption** set to ```noImplicitAny=true```. This will throw compilation error if type is not defined explicitly. To define a variable which expects to have values type - any, explicit declaration of type```: any``` is required.

* Additionally it is a good idea to default initialize parameters of a function to practically make every parameter as optional for the function consumers.



**strictNullCheck**
* ```strictNullCheck``` is a mechanism through which TS prevents many ```null``` or ```undefined``` error during compilation time that usually shows in JS. Hence this is a good recommended practice to keep this turned on in the tsconfig.json. 
* strictNullCheck compiler option must be turned on to ensure any allocation of null or undefined to a variable is flagged as error during compilation
* To allow null or undefined those must be defined explicitly as annotations. Example

```ts
let x: string | null | undefined;
```


**Type Assertions**
* Similar to annotations, but this one is to get a type of variable delayed
```ts
let anyNum: any = 5;
let fixedString: string = (<number>anyNum).toFixed(4);
console.log(fixedString);
```

Alternative syntax:
```ts
let fixedString: string = (anyNum as number).toFixed(4);
```

**Control flow-based type analysis**
* Compiler checks the conditional statement and allows variable to most narrow type element type. Example:
```ts
var messageStrHtml: HTMLElement | string;
if(typeof messageStrHtml == 'string'){
  return messageStrHtml; // in this case TS compiler knows that the variable is type string 
}
else{
  return messageStrHtml; // in this case TS compiler knows that the variable is type HTMLElement 
} 
```


## Arrow Function
* Arrow functions are used for temporary function declared almost similar to how a variable is declared and used immediately
* Left of arrow is parameter, right of arrow is function body
* Even though no return defined, the result is implicitly considered as return value when it is single line arrow function
* Remember: if there is only one parameter to be sent then paranthesis is optional. But if zero or more than one parameters sent, then must have paranthesis. Also single parameters with type annotation must have paranthesis
* If a single line function arrow function does not require any paranthesis to surround the function body, nor does it need a return statement, but if multiline, it must be surrounded by curly braces
* In case of TypeScript though defining return type annotation is required, in case of one line arrow function, it allows not defining return type annotation as it can implicitly figure out the return type. But the parameter must have a type annotation
* Simple Example:

```ts
let squareVal = x => x * x; 

```
* Another example:

```ts
// Regular Function
function logPlayer(name: string = 'Default Player 1'): void {
    console.log(`New game starting player : ${name}`);
}

// converted to arrow function
const logMessageArrow = (message: string) => console.log(message);

// invoke the arrow function
logMessageArrow("Hi There log !");

```


### Passing Function type as annotation
* If multiple functions have same type parameter and same return type then they can be assigned to a variable and can be used conditionally.
* user arrow function to define the function type. Example:

```ts
// define a variable of the type function that accepts one parameter of type string, and does not return any value
let logger: (value: string) => void;

// define a function that accepts one parameter as a string and returns void
function logSomething(value: string): void {
  console.log(`Print something here - ${value}`);
}

// define another function that accepts one parameter as a string and returns void
function logSomethingElse(value: string): void {
  console.log(`Print something else here - ${value}`);
}

// assign a function name based on condition 
if (someCondition){
  logger = logSomething;
} else {
  logger = logSomethingElse;
}

//invoke the conditional function 
logger('Some Message');

//
```


## Understanding Interface 

* Interface declares just the the shape of a class that is - attribute and method signature, classes have the full implementation

Example of interface

```ts
interface ExampleInterface {
  name: string;
  id: number;
  
}

interface ExtendingInterface extends ExampleInterface {
  address: string;
  state: string;
  zip?: number; //this is an optional attribue, notice the ?
  
  //define method with just signature only
  sampleMethod: (paramName: string) => void;
}

```

* There is an implicit match with type structure. This means if two objects have same attributes, they can be typecasted to each other
* This is also called duck type (if it looks like duck, walks like duck, quacks like duck, it must be a duck)`

Example:
```ts
interface FirstInterface {
  name: string;
  id: number;
}

let sampleVariable =  {
  name: "Utkal";
  id: 12345;
  address: "Sparks, MD";
}

let newVar: FirstInterface = sampleVariable;

```

## Understanding Class
* Beyond method implementations, there are more things done with classes and class members such as Access Modifiers (public, private, protected), Accessors (getter & setter)
* Static methods and variables can be simply invoked with class names, no need to create an object

Example:

```ts
class Developer {
  // declare a public variable, no need to explicitly mention public
  department: string;
  
  //declare a private variable, must explicitly mention private
  private _title: string;
  
  // implement getter and setter methods to access private variables
  // remember to add this.var_name to access class members
  get title(): string {
    return this._title;
  }
  set title(newTitle: string): void {
    this._title = newTitle;
  }
}

```
### ECMA script private fields have a slight different way to define private fields in JS supported by TS3.8
```ts
//declare a private variable
# varNAme: string;
```

### How to extend a class

```ts
class GameDeveloper extends Developer {
  // this class inherits all attributes and methods of class Developer
  // from the example above it means - this class also has department attribute
  favoriteEditor: string;
  writeToEdit(): void {
    //do something in the method
    
  }
}

```

### How to implement a interface in a class

```ts
// define new class that implements interface Person
class Player implements Person { 
   
}

// to use this in another ts file, refer it in the following way
const firstPlayer: Player = new Player();
firstPlayer.name = 'UTKAL';
console.log(firstPlayer.formatName());
```

### How to reference to multiple classes from separate ts files
* Add reference to starting ts in the tsconfig file as shown below
```ts
"files": [
        "./app.ts"
]
```

* At the top of the first ts file add reference using the shortcut ```ref``` this produces the following line in VS Studio code
```ts
/// <reference path="person.ts" />
```

* Now to send all output js for the index html to be used, we can send them all bundled into a single js file, using following in the tsconfig

### Constructor in a class
* Role of constructor is to initiatilize attributes of a class as needed
* make sure to call ```super()``` to invoke constructor of the parent class
* if a variable is marked as ```readonly``` it can be only initialized once and within the constructor
* Example of a classic constructor (same as java syntax) below - 

```typescript

class Game {
    private scoreboard: ScoreBoard = new ScoreBoard();
    player: Player;
    problemCount: number;
    factor: number;

    constructor(newPl: Player, numProb: number, multFactor: number){
        this.player = newPl;
        this.problemCount = numProb;
        this.factor = multFactor;
    }
}
```
* **Recommended** Example of a more optimized and reduced way of declaring constructor method - 

```typescript
constructor(public player: Player, public problemCount: number, public factor: number) {}
 
```

## Modules / APIs
* Instead of using references to each used model in ts file using a ///ref syntax module export/ import can be used
* Allow encapsulate implementation details and expose APIs for other modules, this allows refactoring of implementations
* Modules are reusable and provide higher level abstraction
* Note: Unless the compiler is ES2015 then a module loader is needed to run modules. Webpack is an example of Module bundler that helps. Webpack prepares the module run in the browser
* To use it as a module just add the keyword ```export``` at the declaration line of the interface, function or class
* More optimized way of writing export sentence for multiple items at once in the example that follows.

```ts
// example module is person.ts
interface Person {}
function hirePerson(): void {}
class Employee {}
class Manager {}

//export interface, function, class, but keep the Manager class private to the module only
export {Person, hirePerson, Employee as Staff};
```

* consuming modules must import module to use, example -

```ts
// import multiple elements from another module
import { Person, hirePerson } from './person';
let human: Person;

// default module import
import Worker from './person';
let engineer: Worker = new Worker();

// use alias while importing
import { StaffMember as CoWorker } from './person';
let emp: CoWorker = new CoWorker();

// import all the available modules
import * as HR from './person';
HR.hirePerson();

```

* **Best Practice for using Module reference** - For own module use relative path, for third party use non-relative path
* The third party modules are default searched within the node_module package, to alter this use baseurl property in tsconfig

## Type Declaration
* Type declaration is typescript wrapper around popular javascript library
* the file name has extension d.ts. search for these in github repo **DefinitelyTyped**
* Type declaration is a development time tool to show errors during compile time. For runtime the actual javascript files must be deployed
* These can be used using ```npm install``` command


## Destructure Array and Objects
* TypeScript automatically can destructure an array in the following way - 

```ts
let planets = [ 'mercury', 'venus', 'earth', 'mars', 'jupiter', 'saturn'];

let [first, second, third, fourth] = planets;

```
* TypeScript automatically can destructure an Object in the following way - 

```ts
let person = {
  name: 'UTKAL',
  address: '12345 Abcd Street',
  phone: '123-456-7890'
};

let {name, address, phone } = person;
// the new var name must match to that of the attributes of the object
```

### Rest Parameters
* rest parameter is a way to capture the remainder of the elements from an array. Use a variable name preceded with Three dots to declare rest param. 
* Rest params have the same object type as that of the source array. Example: 
```ts
let planets = [ 'mercury', 'venus', 'earth', 'mars', 'jupiter', 'saturn'];

let [first, second, ...oth] = planets;

```

### Spread parameters
* spread is a way to append an array to another array in a simpler 

```ts
let firstArray = [20,30,40];
let secondArray = [1,2,3,4,...firstArray];
console.log();
// prints [1,2,3,4,20,30,40]
```

* **preferred** In case of complex object arrays use ```.push()``` method to spread
```ts
firstArray.push(...secondArray);
// result -> [1,2,3,4,20,30,40]

```

```ts
interface Book {
  id: number;
  title: string;
  author: string;
  available: boolean;
}

let schoolBooks: Book[] = [
  { id: 10, title: "Book Title 1", author: "Author 1", available: true },
  { id: 11, title: "Book Title 1", author: "Author 1", available: true },
  { id: 12, title: "Book Title 1", author: "Author 1", available: true },
];

let collegeBooks: Book[] = [
  { id: 20, title: "Book Title 11", author: "Author 11", available: true },
  { id: 21, title: "Book Title 21", author: "Author 21", available: true },
  { id: 22, title: "Book Title 31", author: "Author 31", available: true },
];

let allBooks: Book[] = schoolBooks;
console.log(allBooks.length);
allBooks.push(...collegeBooks);
console.log(allBooks.length);
```

### Tuple Types
* Like arrays but has a key and value 

```ts
let myTuple: [number, string] = [10, 'Abcde']
// in the above case the first and the second element must be number and the string respectively
// third element onwards can be either type


```

### Union and Intersection types

```ts
// Union -> Can be either of any type specified
// example below shows the id parameter can be string or number
function FindType(id: string | number) {
  // do more
}

// Intersection -> must have properties of both types, mostly applies to objects
function MatchProperties(id: Object1 & Object2) {
  // declare all elements of Object1 and Object2 to qualify
}
```

