# TypeScript-Getting-Started

This is the official repository for my Pluralsight course titled [*TypeScript: Getting Started*](https://app.pluralsight.com/library/courses/typescript-getting-started/table-of-contents). 
The *main* branch contains code as it 
exists at the start of the course. There are separate branches named after the modules in the course that contain the code as it 
exists at the end of that module.

# Utkal Notes 

## Get Started

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
