# What is React and why do we like it?

Hello! Let me guess, you are here because you've made something beautiful using static HTML & CSS and now you want to turn your site/app into a dynamic and interactive one, right?

> From now on, we assume you are familiar with HTML & CSS because those are the foundation of web development. Reason is, it could get too confusing without knowing these first. Actually, this whole page might still be confusing to you, but hang in there, I promise it will eventually make sense and I'll do my best explaining it to you.

## What is React then?

__React is a JavaScript library for building user interfaces.__ But in order to appreciate what it does for us, we'll need to step back for a bit and understand how applications and how user interfaces are built without React.

---

### Functions

When we create applications of any size, we need tools that help us organise the code so we can [easily reason about](https://softwareengineering.stackexchange.com/questions/351244/easy-to-reason-about-what-does-that-mean) it as it grows.
In this tutorial, we'll focus on __functions__ as one of the many tools that helps us organise code.

Let's look at an example: 

__Imagine you want to restrict an action so__

1. only logged in user 
2. and who is an adult 

__can trigger it.__

The code for such thing could look like the following:

_original:_
```javascript
if (!user || user.age < 18) {
  throw new Error("Either not logged in or not an adult");
}
doAction();
```

Now, when you'd want to do the same thing again somewhere else, you'd need to write the same code again (or copy-paste it). And that is... Boooooring.

This is where functions come in. After refactoring the above into a function, our code might look like this:

_refactored:_
```javascript
function isAdult(user) {
  if (!user || user.age < 18) {
    return false;
  }
  return true;
}
```

Then, anywhere in our codebase we can re-use this function just by calling it with a user in hand:

```javascript
if (isAdult(user)) {
  doActionWithUser(user);
}
```

Also, (and excuse the jargon) notice how we got rid of the side-effectful exception throwing, making our code more pure. Making our code pure like this also make it easy to reason about it later or for the new developers coming to work on our application.

---

__Right, let's look at one more, a bit more complex example of a function composition.__

```javascript
React.createElement(
  MainLand,
  {
    open: true
  },
  React.createElement(House, {
    frontDoorOpen: true
  }),
  React.createElement(Garage, {
    lightsOn: false
  })
);
```

What can we say about this code? Well, there are two function calls within one function call. Each function receives two to three arguments and it seems like it has a nested structure. If we know what `React.createElement` does, we might have an idea what it does. But is it easy on the eyes? How long did it take you to figure out the function call flow? You know that the inner function call gets executed before the outer one, right? (And bare in mind, this one is still rather simple)

---

## This is where JXS comes in to play...

You see, the good stuff about HTML is that it provides abstraction that is easy to read. It's a declarative way of describing a user interface. You can say something with a word that means something more complex so that when you markup content with a HTML tag it abstracts away and deals with all the technical details—no need to write a bunch of code.

Based on that idea, guys at Facebook invented something called [XHP](https://www.facebook.com/notes/facebook-engineering/xhp-a-new-way-to-write-php/294003943919/) which was later used as an inspiration for JSX. So what is JSX?

__JSX is a syntax extension__ for Javascript. It allows us to write HTML-like code within Javascript itself.

This is how it looks like:
```javascript
<MainLand open={true}>
  <House frontDoorOpen={true} />
  <Garage lightsOn={false} />
</MainLand>
```

In fact, this code compiles into exactly the same code as we saw previously above. You can see it for yourself [here](https://babeljs.io/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=DwWQhglgdgMmUBMAEB7ADgUygXgN4BcAnAVwwF8A-AKCSWAAkViBnDJAM0JSnwBEUUhAPKYcBEuSQB6arWABxMITABzNgBsIKgBb5mQsezDrWZadWBTw0OIgpA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=true&targets=&version=7.8.7&externalPlugins=).

JSX basically translates into function calls of `React.createElement`, so __React can do some more things with it__ while our application is being rendered.

This way, JSX is more powerful than HTML because the underlying abstraction is "scheduled functions" (we actually call them __"Components"__). And this is the main principle of React. And do you remember I said you came here from the "static HTML" land? Well, components are dynamic, they can have state and something called React scheduler knows when to rerender, remove or add them to the DOM whenever the state changes. This is a big deal.

_Description from the official React documentation:_
> Components are often described as “just functions” but in our view they need to be more than that to be useful. In React, components describe any composable behavior, and this includes rendering, lifecycle, and state. Some external libraries like Relay augment components with other responsibilities such as describing data dependencies. It is possible that those ideas might make it back into React too in some form.

---

## Life before React

Back then when I was a kid (I still am), we used to schedule the rendering ourselves. Using abstractions like jQuery. Here's a concise example of a file that wouldn't be too far from reality ~10 years ago. (Apart from the newer JS syntax like `const` etc.)

_assignment no. x_

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <form>
        <input id="username" name="name" type="text" value="" />
        <input id="password" name="name" type="password" value="" />
        <button id="submit" disabled="disabled">Login</button>
    </form>
    
    <script type="javascript">
        const usernameElementId = "username";
        const passwordElementId = "password";

        const submitBtnEl = document.getElementById("submit");

        function handleChange() {
            const username = document.getElementById(usernameElementId).value;
            const password = document.getElementById(passwordElementId).value;

            if (username.length > 0 && password.length > 0) {
                submitBtnEl.removeAttribute("disabled");
            } else {
                submitBtnEl.setAttribute("disabled", "disabled");
            }
        }

        document.getElementById(usernameElementId).addEventListener("change", handleChange);
        document.getElementById(passwordElementId).addEventListener("change", handleChange);
    </script>
</body>
</html>
```

Without me telling you what the code does (that's your job to figure out), we can already notice a few things:
1. It is vanilla Javascript without any frameworks or abstractions.
1. This code is very __imperative__ and imperative code is very prone to bugs and fragile to modifications.
1. The behaviour is separated from the ui description, if we modify one side of it, we probably need to update the other one, too.
1. It does not cleanup after the form is unmounted. Leaving listeners hanging and creating a memory leak.
1. We have to manually manipulate the DOM whenever we want to change something.

It's worth noting that even if we used jQuery, the code would have the same flaws.

## React

Now, let's look at the same example but using React with the least amount of abstractions.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Login</title>
    <script
      crossorigin
      src="https://unpkg.com/react@16/umd/react.development.js"
    ></script>
    <script
      crossorigin
      src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"
    ></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
  </head>
  <body>
    <div id="app"></div>

    <script type="text/babel">
      function Login() {
        const [username, setUsername] = React.useState("");
        const [password, setPassword] = React.useState("");
        const shouldBeDisabled = username.length === 0 || password.length === 0;
        return (
          <form>
            <input
              name="name"
              type="text"
              onChange={e => setUsername(e.target.value)}
              value={username}
            />
            <input
              name="name"
              type="password"
              onChange={e => setPassword(e.target.value)}
              value={password}
            />
            <button disabled={shouldBeDisabled}>Submit</button>
          </form>
        );
      }

      ReactDOM.render(<Login />, document.getElementById("app"));
    </script>
  </body>
</html>
```

Here's what's happening:
1. We create an empty `div` with id `"app"`
1. We define a component named `Login` where we define the state, behaviour and the UI representation all together.
1. We render the `Login` component into the div we created, so React will start its scheduler and keep track of any changes in that subtree.
1. If we would unmount our component (not part of the code) with `ReactDOM.unmountComponentAtNode(document.getElementById("app"))`, React would cleanup all the attached listeners it has created automatically.
1. The code is __declarative__ and easier to reason about. _(Declarative code is easier to understand thus we prefer declarative code over imperative. In fact, what is very common in React, is to wrap some imperative code with declarative component(s) - we will see an example of this at the end of the webinar.)_

And this is it, this is React. I mean, there's more to it, but let's not put too much on our plate at the beginning. I like to think of React as "Lego for adults". Once you have the building blocks, it's very easy and fast to create application of any scale with it.

If there is anything that doesn't make sense to you, please don't be afraid to ask questions.

Now go and try to fill out the test in the Goole Classroom.