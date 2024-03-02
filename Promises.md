- [Promises](#promises)
  - [.then()](#then)
  - [.catch()](#catch)
  - [.finally()](#finally)
  - [Promise.resolve()](#promiseresolve)
  - [Promise.reject()](#promisereject)
  - [Promise Concurrency](#promise-concurrency)
    - [Promise.all()](#promiseall)
    - [Promise.allSettled()](#promiseallsettled)
    - [Promise.race()](#promiserace)
    - [Promise.any()](#promiseany)
  - [Syntax/Implementation](#syntaximplementation)
    - [Promise syntax](#promise-syntax)
    - [Promise.all](#promiseall-1)
    - [Polyfill of Promise.all()](#polyfill-of-promiseall)


# Promises
[Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

Promise is an object that represents the eventual completion of the asynchronous operation. Promise is a way of handling asynchronous operations. 

![promise](./images/promises.png)

Continue reading you will understand what this diagram represents.

Promise was not always a part of Javascript. Callbacks were used with asynchronous functions to produce desired results in the past. Callback function were passed as arguments to an asynchronous function.

```javascript
function callback(result) {
  // Use the result from the Async operation
}

randomAsyncOperation((response) => callback(response));
```

But it became an issue if the result of the next operation depended on the result of the previous asynchronous operation. This led to the 'Callback pyramid of doom' or also known as 'callback hell'.

Therefore, the Promises was introduced to solve this problem.

A Promise is in one of these states:

    - pending: initial state, neither fulfilled nor rejected.
    - fulfilled: meaning that the operation was completed successfully.
    - rejected: meaning that the operation failed.

![State of Promises](images/promise.png "image_tooltip")

A promise state are irreversible. i.e It cannot go from resolved to rejected state or rejected to resolved state or rejected/resolved to pending state.

The eventual state of a pending promise can either be fulfilled with a value or rejected with a reason (error). When either of these options occur, the associated handlers queued up by a promise's then method are called. If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called.

A promise is said to be settled if it is either fulfilled or rejected, but not pending.

Let's look at the syntax of a promise:

```javascript
const promise = new Promise((resolveFunc, rejectFunc) => {
  // Condition to resolve or reject the promise

  // Perform some asynchronous operation
  const result = performAsyncOperation();
  if(result)){
      // If the operation is successful, resolve the Promise with the result
      resolveFunc(result);
  }else{
      // If there is an error, reject the Promise with an error object
      rejectFunc(new Error('An error occurred'));
  }
});
```

To create a promise, we need to create an instance object using the Promise constructor function. The Promise constructor function takes in one parameter. That parameter is a function called as the executor function. A function to be executed by the constructor. It receives two functions as parameters: resolveFunc and rejectFunc. Any errors thrown in the executor will cause the promise to be rejected, and the return value will be neglected. 

> **_NOTE:_**  resolveFunc and rejectFunc are same as resolve and reject, you can use whatever function name you want but resolve and reject are convention that is commonly used.

So, you might want to perform a task after the promise is resolved or rejected.
Thats when we use .then(), .catch() and .finally() methods.

## .then()
[.then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

The .then() method schedules callback functions for the eventual completion of a Promise — either fulfillment or rejection.

The .then() method of Promise instances takes up to two arguments: callback functions for the fulfilled and rejected cases of the Promise. It immediately returns an equivalent Promise object, allowing you to chain calls to other promise methods called as promise chaining.

How to use then with Promise?

```javascript
const onFulfilled = (value) => {
    console.log(value); 
};

const onRejected = (reason) => {
    console.error(reason); 
};

const p1 = new Promise((resolve, reject) => {
  resolve("Success!");
  // or
  // reject(new Error("Error!"));
}).then(
  onFulfilled,   // this first callback logs if fullfilled:  Success!
  onRejected     // this second callback logs if rejected:  Error!
);
```

You might have seen .then() with single argument right? 
Yes, you are correct.
Thats when catch come into play...


## .catch()
[.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)

The .catch() method of Promise instances schedules a function to be called when the promise is rejected. It immediately returns an equivalent Promise object, allowing you to chain calls to other promise methods. 

It is a shortcut for 
```javascript
.then(undefined, onRejected).
```

Surprised? don't be. 
It is often desirable to .catch() rejected promises rather than .then()'s two-case syntax.

```javascript
const onFulfilled = (value) => {
    console.log(value); 
};

const onRejected = (reason) => {
    console.error(reason); 
};

const p1 = new Promise((resolve, reject) => {
  // resolve("Success!");
  // or
  reject(new Error("Error!"));
}).then(
  onFulfilled
).catch(
    onRejected
);
```

![promise](./images/promises.png)
The above diagram might make sense now.

## .finally()
[.finally](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

The finally() method of Promise instances schedules a function to be called when the promise is settled (either fulfilled or rejected). It immediately returns an equivalent Promise object, allowing you to chain calls to other promise methods.
This lets you avoid duplicating code in both the promise's then() and catch() handlers.

.finally() method takes a function to asynchronously execute when this promise becomes settled. It returns an equivalent Promise. If the handler throws an error or returns a rejected promise, the promise returned by finally() will be rejected with that value instead. Otherwise, the return value of the handler does not affect the state of the original promise.
The finally() method is very similar to calling .then(onFinally, onFinally). 

```javascript
function checkMail() {
  return new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
      resolve('Mail has arrived');
    } else {
      reject(new Error('Failed to arrive'));
    }
  });
}

checkMail()
  .then((mail) => {
    console.log(mail);
  })
  .catch((err) => {
    console.error(err);
  })
  .finally(() => {
    console.log('Experiment completed');
  });
```

We can create an immediately resolved promise.

## Promise.resolve()
[Promise.resolve()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

The Promise.resolve() static method "resolves" a given value to a Promise. If the value is a promise, that promise is returned; if the value is a thenable, Promise.resolve() will call the .then() method of the thenable object with two callbacks it prepared; otherwise the returned promise will be fulfilled with the value.
In summary:
 - If a non-thenable value is passed, the returned promise is already fulfilled with that value.
 - If a thenable is passed, the returned promise will adopt the state of that thenable by calling the then method and passing a pair of resolving functions as arguments. (But because native promises directly pass through Promise.resolve() without creating a wrapper, the then method is not called on native promises.) If the resolve function receives another thenable object, it will be resolved again, so that the eventual fulfillment value of the promise will never be thenable.
  
***
> **_NOTE:_** “thenable” is an object or function that defines a then method. All promises are thenable but not all thenable are promises.
```javascript
// A thenable is an object with a `then()` function. The
// below thenable behaves like a promise that fulfills with
// the value `42` after 10ms.
const thenable = {
  then: function(onFulfilled) {
    setTimeout(() => onFulfilled(42), 10);
  }
};

Promise.resolve().
  then(() => thenable).
  then(v => {
    v; // 42
  });
```
***

## Promise.reject()
[Promise.reject()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)
The Promise.reject() static method returns a Promise object that is rejected with a given reason.
The static Promise.reject function returns a Promise that is rejected. For debugging purposes and selective error catching, it is useful to make reason an instanceof Error.
```javascript
function resolved(result) {
  console.log('Resolved');
}

function rejected(result) {
  console.error(result);
}

Promise.reject(new Error('fail')).then(resolved, rejected);
// Expected output: Error: fail
```

Unlike Promise.resolve(), Promise.reject() always wraps reason in a new Promise object, even when reason is already a Promise.

```javascript
const p = Promise.resolve(1);
const rejected = Promise.reject(p);
console.log(rejected === p); // false
rejected.catch((v) => {
  console.log(v === p); // true
});
```


## Promise Concurrency
The Promise class offers four static methods to facilitate asynchronous task concurrency

> **_NOTE:_** JavaScript is single-threaded by nature, so at a given instant, only one task will be executing, although control can shift between different promises, making execution of the promises appear concurrent. Parallel execution in JavaScript can only be achieved through worker threads.

### Promise.all()
[Promise.all()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

Fulfills when all of the promises fulfill; rejects when any of the promises rejects.
Takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when all of the input's promises fulfill (including when an empty iterable is passed), with an array of the fulfillment values. It rejects when any of the input's promises reject, with this first rejection reason.

### Promise.allSettled()
[Promise.allSettled()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

Fulfills when all promises settle.
Takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when all of the input's promises settle (including when an empty iterable is passed), with an array of objects that describe the outcome of each promise.

### Promise.race()
[Promise.race()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

Settles when any of the promises settles. In other words, fulfills when any of the promises fulfills; rejects when any of the promises rejects.
Takes an iterable of promises as input and returns a single Promise. This returned promise settles with the eventual state of the first promise that settles.

### Promise.any()
[Promise.any()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

Fulfills when any of the promises fulfills; rejects when all of the promises reject.
Takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when any of the input's promises fulfill, with this first fulfillment value. It rejects when all of the input's promises reject (including when an empty iterable is passed), with an AggregateError containing an array of rejection reasons.

The AggregateError object represents an error when several errors need to be wrapped in a single error. It is thrown when multiple errors need to be reported by an operation.
AggregateError object contains an array of rejection reasons in its errors property.



## Syntax/Implementation

### Promise syntax
```javascript
const promise = new Promise((resolve, reject) => {

    // setTimeout(() => {
    //     resolve('promise resolved'); 
    // }, 1000)

    reject(new Error('promise rejected'));

})

promise.then(
    (result) => {
        console.log(result);
    },
    // (err) => {
    //     console.log('error ', err); // second parameter of then
    // }
).catch((err) => {
    console.log('error catched', err)
}).finally(() => {
    console.log('finally')
})
```


### Promise.all

```javascript
const promise1 = new Promise((resolve, reject) => {
    resolve('resolved1');
    reject('reject1')
})

const promise2 = new Promise((resolve, reject) => {
    // resolve('resolved2');
    reject('reject2')
})

const promise3 = new Promise((resolve, reject) => {
    resolve('resolved3');
    reject('reject3')
})

const promise4 = new Promise((resolve, reject) => {
    resolve('resolved4');
    reject('reject4')
})

const promise_all = Promise.all([promise1, promise2, promise3, promise4]).then((results) => {

    results.forEach((result) => {
        console.log(result)
    })

},
    //     (error) => {
    //     console.log(error);// second parameter of then
    // },
).catch((error) => {
    console.log(error)
})
```


### Polyfill of Promise.all()

   - let's learn step by step... 

```javascript
const dummyPromise = (timeout) => { // to simulate an asynchronous operation
    return new Promise((resolve, reject) => {
        if (timeout === 0) {
            reject('rejected ' + timeout);
        }
        setTimeout(() => {
            resolve(timeout);
        }, timeout);
    })
}


Promise.all([dummyPromise(1000), dummyPromise(0), dummyPromise(3000)]).then((data) => {
    console.log(data);
}).catch((err) => {
    console.log(err)
})
```

    - Promise.all() basically takes an array of promises and returns a new promise with fullfilled values (array) or rejected reason.

```javascript
const CustomPromise = (arrayOfPromise) => {
    return new Promise((resolve, reject) => {

    })
}
```
    - loop over the given array of promises.

```javascript
const CustomPromise = (arrayOfPromise) => {
    return new Promise((resolve, reject) => {
            arrayOfPromise.forEach((promise,index)=>{
                promise.then((result)=>{
                        
                })
            })
    })
}

```

    - Promise.all returns an array of results if all promises are fullfilled else returns the first encountered rejection

```javascript
const arrayOfPromise = [dummyPromise(1000), dummyPromise(0), dummyPromise(3000)];

const customPromise = (arrayOfPromise) => {
    let resultsArray = []; //to store the resolved promises
    return new Promise((resolve, reject) => {
        arrayOfPromise.forEach((promise, index) => {
            promise.then((result) => {
                resultsArray[index] = result;
                if (arrayOfPromise.length - 1 === index) { // checks if all promises are settled
                    resolve(resultsArray);
                }
            }).catch((error) => {
                reject(error)
            })
        })
    })
}

```


    - Some edge case - if given array contains zero element, if elements are non-Promise values.

```javascript
// empty array
const arrayOfPromise = []; 

// non-Promise values
// const arrayOfPromise = [1, 2, 34]

const customPromise = (arrayOfPromise) => {
    let resultsArray = [];
    return new Promise((resolve, reject) => {
        if (arrayOfPromise.length === 0) resolve(arrayOfPromise);// if array contains no element

        arrayOfPromise.forEach((promise, index) => {
             if (!(promise instanceof Promise)) {// if element is not an instance of Promise
                resultsArray[index] = promise;

                if (arrayOfPromise.length - 1 === index) {
                    resolve(resultsArray);
                }
            } else {
                promise.then((result) => {
                    resultsArray[index] = result;
                    if (arrayOfPromise.length - 1 === index) { // checks if all promises are settled
                        resolve(resultsArray);
                    }
                }).catch((error) => {
                    reject(error)
                })
            }
        })
    })
}
```


    - let's check our implementation
  
```javascript

// check for resolved results

const arrayOfPromise = [dummyPromise(1000), dummyPromise(4000), dummyPromise(1000)];

//check for rejection

// const arrayOfPromise = [dummyPromise(1000), dummyPromise(4000), dummyPromise(0)];

// check for non-Promise values

// const arrayOfPromise = [1, 2, 34]

const customPromise = (arrayOfPromise) => {
    let resultsArray = [];
    return new Promise((resolve, reject) => {
        if (arrayOfPromise.length === 0) resolve(arrayOfPromise);

        return arrayOfPromise.forEach(async (promise, index) => {

            if (!(promise instanceof Promise)) {
                resultsArray[index] = promise;

                if (arrayOfPromise.length - 1 === index) {
                    resolve(resultsArray);
                }
            } else {
                promise.then((result) => {
                    resultsArray[index] = result;

                    if (arrayOfPromise.length - 1 === index) {
                        resolve(resultsArray);
                    }
                }).catch((error) => {
                    reject(error)
                })
            }
        })
    })
}


customPromise(arrayOfPromise).then((data) => {
    console.log(data)
}).catch((err) => {
    console.log(err)
})
```


# Important to know-

One important detail is that the executor function passed to the Promise() constructor is called immediately (before the constructor returns the promise); whereas the handler functions passed to the then() method will not be called till later (if ever).

__resolve() is not onFulfill()__

One more detail I'd like to emphasize, is that the resolve() and reject() callbacks passed to the Promise() constructor's executor function are not the callbacks later passed to the then() method. There is definitely a connection, but it's a loose, dynamic one.

Instead, the resolve() and reject() callbacks are functions supplied by the "system", and are passed to the executor function by the Promise constructor when you create a promise. When the resolve() function is called, system code is executed that potentially changes the state of the promise and eventually leads to an onFulfilled() callback being called asynchronously. Don't think of calling resolve() as being a tight wrapper for calling onFulfill()!

In short the executor function passed to the promise constructor is executed synchronously.
and when the promise is resolved its time to execute the then function, it puts the callback of then function to the microtask queue which is watched by the event loop to put into the call stack when call stack is empty.
and mean while rest of the codes are executed synchronously.



## Here's a breakdown of the key points:

- Executor Function: The executor function passed to the Promise constructor is executed synchronously. This means that any code inside the executor function is executed immediately when the Promise is created.

- resolve() and reject(): The resolve() and reject() functions are not the same as the onFulfilled() and onRejected() callbacks passed to the then() method. Instead, they are functions provided by the JavaScript runtime environment. When resolve() is called, it triggers the execution of the onFulfilled() callback passed to then().

- Microtask Queue and Event Loop: When a Promise is resolved, the then() callback is added to the microtask queue. The event loop continuously checks the microtask queue for tasks to execute. When the call stack is empty, the event loop processes the microtask queue and executes the then() callback.

- Synchronous Execution: While the then() callback is waiting in the microtask queue, the rest of the code continues to execute synchronously. This means that any code after the then() method is executed before the then() callback is executed.


