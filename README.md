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

