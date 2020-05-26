# Svelte Boilerplate with Example
  This is a boilerplate for custom element build with svelte. I have some additional code to demonstrate how to use dispatch events cause that varies based on the normal compile to custom element. You can see the "Clean" Section below on how to remove those code.


## Get Started

  1. Fork this repo. click on the fork button on the top in github
  2. Clone / download the forked repo ```git clone <your repo url>```
  3. run ```npm install```
  4. run ```npm run dev``` the app should be running in port 5000

## Stack
1. Prettier ( to format the code )
2. Rollup ( to compile the svelte to js )

## Setup Explation

### 1. Roll up Configuration in ```/rollup.config.js```
 In Rollup set the custome Element to true, <br>
 This will compile to custom element not for SPA, the output will be one JS file stored in either dist or build
 
``` 	
  customElement: true
```
### 2. Index HTML in ```/public/index.html```
 - Added a script which point to builded file stored in build/bundle.js
 
 ```
	<script defer src='/build/bundle.js'></script>
 ```

 - Also imported a web componenet "hello-component" 

 ```
	<hello-component></hello-component>
 ```
 -  Rest of the code is to demonstrate how to dispatch the event in svelte, that can be listened to index.html

 ### 3. main.js in ```/src/main.js```

  - Instead Targeting the svelte we are just exporting the svelte as App and use html tag ```<hello-world></hello-world>``` to import in the ```public/index.html``` file

```
  export { default as App } from "./App.svelte";
```

### 4. package.json in ```/package.json``` 

  - added a new script ```dist``` to convert the svelte file to one single js file and store it in the ```/dist/index.js```
  Which can be pushed to github.

```
  "dist": "rollup -c -o dist/index.js"
```

### Note:
I have some additional code to demonstrate, how to dispatch the event from a web component and listen to that event in the site. 

Svelte have an issue dispatching an event for custom elemnt, you can read more about it [here](https://github.com/sveltejs/svelte/issues/3119).<br>
Svelte is aware of that and working on it.<br>
but for now we have a work around thanks to "TehShrike" .so, instead of dispatching the event like this 

 ```
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  
  dispatch("eventName", "event optional data");

 ```

 > We have to do this in custom element.

 ```
  import { createEventDispatcher } from "svelte";
  import { get_current_component } from "svelte/internal";

  const component = get_current_component();
  const svelteDispatch = createEventDispatcher();
  
  const dispatch = (name, detail) => {
    svelteDispatch(name, detail);
    component.dispatchEvent &&
      component.dispatchEvent(new CustomEvent(name, { detail }));
  };

  dispatch("eventName", "event optional data");
 ```

if you want to clean up this code, follow Clean up instruction below.

### Clean Up
Let's remove the demonstration code. so, you can write your own.


1. Delete ```/src/SubComponent.svelte``` file
2. Remove the code inside the body in ```/public/index.html```
3. Remove all the code in the ```/src/App.svelte``` except this line 

```
<svelte:options tag="hello-component" />
```
4. Change the tag name and also add the tag in the index.html and run npm run dev start developing your widget. Enjoy
5. Clear the /README.md file

### Thank you
Hope this is useful, Happy coding



