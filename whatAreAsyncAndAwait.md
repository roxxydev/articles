# What are Async and Await?

You might have seen code like the following

```jsx
async function getName() { return "Juan" };
const name = await getName();
console.log(`Hello ${name}, how are you doing?`);

// Prints to console: Hello Juan, how are you doing?
```

and you maybe wondering what are those `async` and `await` keyword? So basically these async and await keyword were added in ECMAScript 2017. The **async** keyword makes the function return a `Promise` while the **await** keyword makes the function wait for the Promise.

...yeah.. but what about **Promise**?

So before we dive deeper about async and await. We’ll go first on getting to know what a Promise is. The Promise by definition from official MDN Web Docs, it is the object that *represents the eventual completion (or failure) of an asynchronous operation and its resulting value.*

---

## Multi-threading and Asynchrony

As we all know Javascript is single threaded. When we say by single threaded it means that no two scripts can at the same time. A code must be finished executing first before it move to the next piece of code.  So how does javascript achieve things asynchronously when it is generally that it is single thread. You can’t run two things in parallel in a single threaded, if you do so then that is where threading comes in. In a multi threaded application, you can run two pieces of code in different threads that can execute in parallel. Do not confuse yourself when we say asynchronously and multi-threading. When two pieces of code can run in parallel, it doesn’t mean that its multi-threadings. Sometimes it just runs the code asynchronously and this is how Javascript does it using Promise. When we say multi-threading, it means that the code runs in parallel on multiple threads. But when we say asynchronously just means that code running doesn’t block the thread and instead it just have a registered callback where you’ll be notified when the code operation has finished.

**Multi-threading**:

*Okay I’ll clean the room. You wash the dishes.*

**Asynchronously:**

Hey son, y*ou go drive to the store and buy tomato sauce. Let me know when you’re back so we’ll cook the spaghetti sauce. Meanwhile, I’ll prepare the ingredients.*

This how Javascript makes it look like multi-thread even though it’s not, through Promises. Basically Promise is somewhat an object that represents the result that will be returned at some point in future. Either a failure or a success result can be returned by the Promise. In the example we said in asynchronously, we can’t guarantee when the son will get back but we do know that he’ll either have a tomato sauce bought or none at all, indeed success or failure.

---

## Promise states

**A Promise basically have 3 states.

*(from MDN Web Docs)*

> A Promise is in one of these states:

- *pending*: initial state, neither fulfilled nor rejected.
- *fulfilled*: meaning that the operation was completed successfully.
- *rejected*: meaning that the operation failed.
>

So let’s take a look a Promise example below

```jsx
const buyTomatoSauce = new Promise((resolve, reject) => {
  if (hasTomatoSauceAvailable) {
    const sauceBought = 'Del Monte Tomato Sauce';
    resolve(sauceBought);
  } else {
    const sauceOutOfStock = 'No stock of tomato sauce';
    reject(sauceOutOfStock);
  }
})
```

You’ll see that there’s these `resolve` and `reject` parameter in the Promise object. These resolve and reject parameter are actually callbacks. These let the caller of the Promise know the resulting state of it. The fulfilled state happens when the Promise resolve callback gets called. While the rejected state is when the reject callback gets called. The Promise is in state of pending when it is yet to be resolve or rejected.

Executing the Promise we have will yield to the following code example

```jsx
buyTomatoSauce()
.then(data => console.log(data),
	reason => console.log(reason));
```

You’ll notice the `then` keyword keyword when we called the `buyTomatoSauce`. So what is that `then` keyword? The *then* keyword basically accepts two arguments which are callbacks for on fulfillment and rejection of the Promise. The return object of the *then* keyword is also a Promise.  So what happens in our code above is that the Promise buyTomatoSauce gets executed and it’s result will be passed to the then. If Promise yields to success then the first callback of *then* gets executed but if it yields to rejection then it yields to the second callback. In case the Promise throws an error *then* gets rejected and throw an error. To catch the error you can then chain it to `catch` which will deal with the rejection of the Promise.

```jsx
buyTomatoSauce()
.then(data => console.log(data),
	reason => console.log(reason))
.catch(err => console.log(err));
```

You can also chain the `finally` method to the Promise. What *finally* does to the Promise is that the callback parameter in the *finally* gets executed when the Promise is settled regardless of what stated it is, be it fulfilled or rejected. Take a look at the example we have below.

```jsx
buyTomatoSauce()
.then(data => console.log(data),
	reason => console.log(reason))
.catch(err => console.log(err))
.finally(() => {
	console.log("Promise finished!");
});
```

The “Promise finished” gets printed to the console regardless if the Promise was fulfilled or rejected.

---

## Async/Await

Async/await is  extension of promises we got in Javascript. So why have **async** and **await** when we already have these approach in Promise like the *then, catch, and finally*?

Let’s take a look at the example called below

```jsx
buyTomatoSauce()
.then(data => {
		console.log("What is the tomato sauce? ", data);
		return data;
	},
	reason => {
		console.log(reason);
	})
.then(tomatoSauceBrand => {
		if (tomatoSauceBrand === "Del Monte") {
			console.log("Let's cook dinner now.");
		}
		else {
			throw new Error("Incorrect brand of tomato sauce!");
		}
	},
	reason => {
		console.log(reason);
	})
.catch(err => console.log(err))
.finally(() => {
	console.log("Promise finished!");
});
```

You’ll notice that we have a chained multiple `then` . There’s nothing really about it but once you got lot of sequential, asynchronous thing going on and all of which are chained then you’ll probably end up boilerplate of chained Promises. This is where async/await comes. It doesn’t necessarily replace how you create Promises but rather it improves the way how you write asynchronous code. So transforming the code with chained *then,* we’ll have a much cleaner code which also helps you avoid possible logic nightmare tracing through each promises of possible cause of error or issue.

```jsx
async function cookDinner() {
	const brandOfTomatoSauceBought = await buyTomatoSauce();
	console.log("What is the tomato sauce? ", data);

	if (tomatoSauceBrand === "Del Monte") {
		console.log("Let's cook dinner now.");
	}
	else {
		throw new Error("Incorrect brand of tomato sauce!");
	}
}

async function prepareDinner() {
	try {
		await cookDinner();
	} catch (err) {
		console.log(err);
	}
}
```

A much readable, straightforward, and cleaner approach isn’t it? Any error within the asynchronous function *cookDinner* will get log in the *catch* inside the *prepareDinner* function. You can also throw error inside the asynchronous function and since the *cookDinner* is a Promise because of the *async* keyword, it then waits for the result. Since we throw an error, the *await cookDinner()* yields to a Promise rejection because of the error thrown which will then be handled in the *catch*. As mentioned in the beginning that **async** keyword makes the function return a `Promise` while the **await** keyword makes the function wait for the Promise. The *cookDinner* is always guaranteed to return a Promise regardless of whatever object returned that you wrap inside the async function. The *await* doesn’t necessarily block the thread by just wait for the results of the Promise. So instead of you creating callbacks in Promise *then*, you can just use await to directly wait for the Promise result.

So that’s it! Hope you now get a grasps of what Promises are and how Async/Await works.
