# TypeScript-Getting-Started

This is the official repository for my Pluralsight course titled [*TypeScript: Getting Started*](https://app.pluralsight.com/library/courses/typescript-getting-started/table-of-contents). 
The *main* branch contains code as it 
exists at the start of the course. There are separate branches named after the modules in the course that contain the code as it 
exists at the end of that module.

# Utkal Notes 

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



**Annotations**
* Annotations are used to declare type of variable. Though it is not mandatory, compiler forces type by default during initialization. Annotation makes it explicit. Example:

```ts
let x:string = "Example of a String !"
```
* Types can also be of union type to allow multiple type of values. Example:

```ts
let x: string | number ;
```

**Annotations in Function**
* JS considers all parameters to function as optional, allows more than declared number of parameters as well as any type. In TS it is considered required and recommended to declare type of parameter. To make a parameter optional use '?'. Example:

```ts
function functionName(param_name: number, param_name?: string): string {
  //function body here
}
```

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


