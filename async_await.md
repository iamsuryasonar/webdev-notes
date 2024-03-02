# Async/Await

- async and await are keywords in javascript,
- It is a way to handle asynchronous operations, just like Promise and Callback functions,
- It is a syntactical sugar over the implementation of Promise,
- It allows us to implement asynchronuous operations as if it is a synchronous operation,
- awaits can be used only inside an async function or in top level of a module(Top level await).
- async function always returns a Promise object.
- It might look like it blocks the main thread but it doesn't.
- await should be used before a function that returns a promise. 
  
    ```javascript
    async function fetchData(){ //normal function
        await someFuntionThatReturnsPromise();
    }

    const fetchData = async()=>{ //arrow function
        await someFuntionThatReturnsPromise();
    }

    ```
Let see with some code...

```javascript
const fetchData = () => {
    // this is a function to simulate a asynchronous operation
    return new Promise((resolve, reject) => {
        setTimeout(() => { 
            resolve('Data fetched successfully');
        }, 2000)
    })
}

// async function
const getData = async () => {
    const data = await fetchData(); // fetchData is an asynchronous operation, meaning we will get result in future.
    return data; // this data returned will always be a promise object
}

console.log('get data...');

//Invoke the getData funtion, that will return a promise object, but how would we handle that promise object? Yes using .then() method

getData().then((result) => {
    console.log(result);// output: Data fetched successfully
})


console.log('see call stack is not blocked!!!')

// output:
// get data...
// Data fetched successfully
// see call stack is not blocked!!!
```

From the above example you can see that call stack is not blocked.
Let's check the flow of control-

- We know javascript is executed from top to bottom.
- fetchData and getData are initialised.
- get data... is logged to the console.
- getData() function is invoked,
- control moved to the function getData
- since it is a async function and it has an await keyword, which is awaiting an asynchronous operation, the async function is suspended from the call stack and runtime takes it from there and executes it is different thread.
- In the meantime, the control moves to the next statement, after the function invocation. Because the call stack is free to execute next task, it is not blocked.
- 'see call stack is not blocked!!!' is logged to the console.
- After the asynchronous operation is completed executing, the runtime puts the suspended function and the data received from asynchronous operation to the micro-task queue.
- We know that the event loop will place task from the micro-task queue to the call stack if the call stack is empty in First in First out order.
- Since the call stack was empty, we can see the. .then() is executed with the result it got from the asynchronous operation.
  ```javascript
    getData().then((result) => {
        console.log(result); // output: Data fetched successfully
    })
  ```

Now that you got the hang of it let's try to implement async await in real world problem-

```javascript
// async function
const getData = async () => {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
    const data = await response.json();
    return data;
}

console.log('get data...');

getData().then((result) => {
    console.log(result);
})

console.log('see call stack is not blocked!!!')

// output:
// get data...
// see call stack is not blocked!!!
// { userId: 1, id: 1, title: 'delectus aut autem', completed: false }

```


# The only difference is the execution context between promise and async/await.

When a Promise is created and the asynchronous operation is started, the code after the Promise creation continues to execute synchronously. When the Promise is resolved or rejected, the attached callback function is added to the microtask queue. The microtask queue is processed after the current task has been completed but before the next task is processed from the task queue. This means that any code that follows the creation of the Promise will execute before the callback function attached to the Promise is executed.

On the other hand, with Async/Await, the await keyword causes the JavaScript engine to pause the execution of the async function until the Promise is resolved or rejected. While the async function waits for the Promise to resolve, it does not block the call stack, and any other synchronous code can be executed. Once the Promise is resolved, the execution of the async function resumes, and the result of the Promise is returned. If rejected, it throws an error value.