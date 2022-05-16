# conceptual-video-
1 What is EventLoop? Explain in brief?
Browser attaches all the Web APIs to global space/object (window object) and this gives them access to execution stack;
Some of the common Web APIs are DOM API, fetch API, web storage APIs (like local storage etc), setTimeout etc
example 1
Now let’s try and understand event loops with this example;
console.log("Before setTimeout")

setTimeout(function printHello(){
  console.log("hello")
},5000)

console.log("After setTimeout")
 
when we start executing the above code; The Global Execution Context gets inside the execution stack. After memory allocation phase; Code execution phase starts and prints “Before setTimeout” to console
When the browser sees setTimeout it registers the callback (printHello) that has been passed inside this setTimeout function and also note down the delay time. timer starts in browser
The execution context moves to next line and prints “After setTimeout”
Once we reach the end of code. The Global Execution Context gets popped off from the execution stack.
Now Once the timer expires, this callback function printHello(){} needs to go to the execution stack
But this can’t be directly go to execution stack. It’ll go to the execution stack through Callback Queue
Now this is where Event Loops come into picture. The job of the Event Loop is to check Callback Queue and Execution Stack and if there exists any callback function inside Callback Queue and if Execution Stack is Empty. If callback function inside Callback Queue exists and if execution stack is empty, It puts the callback function inside Callback Queue into Execution Stack.


2 Flexbox vs Grid system
CSS Grid Layout is a two-dimensional system, meaning it can handle both columns and rows, unlike flexbox which is largely a one-dimensional system (either in a column or a row).

A core difference between CSS Grid and Flexbox is that — CSS Grid’s approach is layout-first while Flexbox’ approach is content-first. If you are well aware of your content before making layout, then blindly opt for Flexbox and if not, opt for CSS Grid.

Flexbox layout is most appropriate to the components of an application (as most of them are fundamentally linear), and small-scale layouts, while the Grid layout is intended for larger-scale layouts which aren’t linear in their design.

If you only need to define a layout as a row or a column, then you probably need flexbox. If you want to define a grid and fit content into it in two dimensions — you need the grid.

3 callback promises/ async await, what is Promise.all?/Explain promises to a 5 year old, with simple examples


Promises are Commitment by someone to do or not to do something;
Now let’s take an example;
function appendScript(externalScript) {
  let script = document.createElement("script");

  script.src = externalScript;

  document.head.append(script);

  /*
    1. here we are trying to load an external script and then invoke a function hello from that script;
  */

  hello(); // 2. this particular function is dependant on script to get completely loaded --> dependant on some other piece of code; --> this is where promises come in
}

appendScript("/myscript.js");
 
basically in the above case the script should give us a promise that when i am done loading --> you can load ‘myscript.js’ and execute the function;
Now let’s understand Why Promises ??
Sometimes a piece of code will take ‘x’ seconds to execute.
Depending on the it, we have other piece of code.

This other piece of code is dependent on first piece of code. So, it waits until first piece of code is done executing
 
basically we want some mechanism where we want some signal that in above case for example that we get a signal to invoke this function hello();
Promises have consequences.
They will be fullfilled.
They will not be fullfilled.
They will be in the process.
Promise by nature, takes time to fulfill or not fulfill.
Basically, we have to WAIT.
In JS programming, We need asynchronous behaviour.
Promises are useful when we have to wait for return value and use that return value without interrupting anything else in our program.
Promise is an object ( constructor object )
var myPromise = new Promise(function (resolve, reject) {
  resolve(`successful`);

  reject(`got rejected`);
});

console.log(`myPromise : `, myPromise); //if resolved --> Promise {<fulfilled>: 'successful'}
console.log(`myPromise : `, myPromise); //if rejected --> Promise {<rejected>: 'got rejected'}
 
modifications to the above code;
function appendScript(s) {
  return new Promise(function (resolve, reject) {
    let script = document.createElement("script");

    script.src = s;

    document.head.append(script);
    script.onload = function () {
      resolve(`Script loading done...`);
    };

    script.onerror = function () {
      reject(`Script loading failed`);
    };
  });
}

appendScript("./myScript.js")
  .then(function (res) {
    console.log(res);
    hello() // invoke function only when script is loaded
  })
  .catch(function (err) {
    console.log(err);
  });
 
Syntax for promises;
myPromise.then(function (res) {
  // do something when promise is successful
})

myPromise.catch(function (err) {
  // do something when promise is a unsuccessfull
})
 
Now for the sake of explanation; we’ve tried to simplify; But if you are looking for official documentation. we’d recommend go through https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

Write a function called sleep that will return a promise, if you do not provide a number to the function, then it will return an error and goto the catch block
function sleep(timer){
  return new Promise(function(resolve,reject){
    if(timer && typeof timer==='number'){
      setTimeout(()=>{
        resolve(`slept for ${timer} milli seconds`)
      },timer)
    }
    else{
      reject(`Timer value is missing..`)
    }
  })
}
 
  
4 ) var, let and const?what are the differences, and how do scopes matter for each?  

var variables can be updated and re-declared within its scope; 
let variables can be updated but not re-declared within its scope 
const variables can neither be updated nor re-declared.

// let se with example 
var a;
a = 10;
b = a; // var are able to reintialized as well as update 
console.log(b)

let c;   // var are able to reintialized as well as update 
c = 20;
c = 40;
c = c;
console.log(c)
 there are two type of scopping in javascript is 
1) Global scope 2) local Scope/Function scope 

// this code shows that var a have golbal scope
 // declearation of variable
console.log(a);
const a = 10   // intaliozation  // undfine with var cant acesses with let const

function add() {
    console.log(a + a);
    let c = 30; // local scope or function scope 
    console.log(c)
}
add(10)
  
 5 Redux architecture

explain redux to a five year old?

what are thunks? why do you need them?

what are action creators?

what are reducers?
  
Redux architecture 
 Redux is global state managment tool is use manage state in react in react componnet we want some data in difrent componenet and 
  componet is also a nested compnenet so we have to pass data from function 1 to function 2 and nth function in componet hierachy 
  in raect provide context api but iyts not effiecient solution if we have 50,0000 componet how we manage our compone state cpnytext api
  not solution so we use redux as state managment tool 
  in simple word we store our backend datat in redux store and use whenevr we want 
  
  1 action what ction we want reducx data 
  ex for adding -product 
  2 Reducer  in which formayt we want data in array or object we reducer for thgat 
  3 step on the basis of that action we make our function and use it 
  for sending the data to redux store we use --- useDispatch function  dispatch(user) in useEffect 
  for getting data back we use useSelector and property of data like const {user} = useSlector((state)=>(state.user))

  6 Stateful vs Stateless Components? 
  They are also known as:

– Container vs Presentational components

– Smart vs Dumb components
  The literal difference is that one has state, and the other doesn’t. That means the stateful components are keeping track of changing data, while stateless components print out what is given to them via props, or they always render the same thing.

7 : explain the difference when working directly with DOM
  What are the benefits of using DOM?
Image result for explain the difference when working directly with DOM
Advantages. By manipulating the DOM, you have infinite possibilities. You can create applications that update the data of the page without needing a refresh. Also, you can create applications that are customizable by the user and then change the layout of the page without a refresh.
  
  8 what is reconciliation and react fibre?
  Reconciliation is the algorithm React uses to diff one tree with another to determine which parts need to be changed.

Update is a change in the data used to render a React app. Usually the result of `setState`. Eventually results in a re-render.
  React Fiber is an ongoing reimplementation of React’s core algorithm. It is the culmination of over two years of research by the React team.

The goal of React Fiber is to increase its suitability for areas like animation, layout, and gestures. Its headline feature is incremental rendering: the ability to split rendering work into chunks and spread it out over multiple frames.
  
  9 what is react state management?
 State of a component is an object that holds some information that may change over the lifetime of the component. We should always try to make our state as simple as possible and minimise the number of stateful components.
const [state, setState] = React.useState( )
  
10 what is the difference between redux and context api?
  The simplest way to pass data from a parent to a child in a React Application is by passing it on to the child's props. But an issue arises when a deeply nested child requires data from a component higher up in the tree. If we pass on the data through the props, every single one of the children would be required to accept the data and pass it on to its child, leading to prop drilling, a terrible practice in the world of React.

To solve the prop drilling issue, we have State Management Solutions like Context API and Redux. But which one of them is best suited for your application? Today we are going to answer this age-old question!
  
  What is the Context API?
Let's check the official documentation:

In a typical React application, data is passed top-down (parent to child) via props, but such usage can be cumbersome for certain types of props (e.g. locale preference, UI theme) that are required by many components within an application. Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree.
  So What is Redux?
Of course, let's head over to the documentation:

Redux is a predictable state container for JavaScript apps.

It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time-traveling debugger.

You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.
  
  MongoDB vs MySql,
  MySQL is a relational database management system (RDBMS) from the 
Oracle Corporation. Like other relational systems, MySQL stores data 
in tables and uses structured query language (SQL) for database 
access. When MySQL developers need to access data in an application, 
they merge data from multiple tables together in a process called a 
join. In MySQL, you predefine your database schema and set up rules 
to govern the relationships between fields in your tables.
  
  MongoDB is a NoSQL database that stores data as JSON-like documents. 
Documents store related information together and use the MongoDB 
query language (MQL) for access. Fields can vary from document to 
document - there is no need to declare the structure of documents to 
the system, as documents are self-describing. Optionally, schema 
validation can be used to enforce data governance controls over each 
collection.
  
  <h1> video link </h1>
  
 https://user-images.githubusercontent.com/93374701/168640505-fa785d8b-6286-4e98-b0a2-3f7d8f763a8f.mp4


  
  
  
  
  
  
  
  
  
  
  
