# State

So our app is now written using React. But it's still not very interactive, is it? In this lesson, we learn about how the state is managed within React apps.

What is a state you ask? Well, that's a mouthful question. For this lesson, I'm just going to say that state is something that can change when a user interacts with our app, something that's not hardcoded as we did in [lesson 1](./lesson1.md).

A quick note before we jump in: There is one more way to manage state, using classes but we won't be covering that because the industry is now shifting towards the hooks way anyway.

## Hooks

Ok, let me just show you how it's done and I'll try to explain how it works at the end of this lesson.

Let's add our menu to the `./src/App.js`, I've prepared the JSX code below for this so you can just copy-paste it.

```jsx
import React from "react";

import Home from "./pages/Home"; 
import Work from "./pages/Work"; 
import Contact from "./pages/Contact";


import "./styles/main.scss";

export default function App() {
  const activeSection = "home";

  return (
    <div id="wrapper">
      <nav id="nav">
        <a href="#home" className={activeSection === "home" && "active"}> 
          <i className="icon solid fa-home" />
          <span>Home</span>
        </a>
        <a href="#work" className={activeSection === "work" && "active"}>
          <i className="icon solid fa-folder" />
          <span>Work</span>
        </a>
        <a href="#contact" className={activeSection === "contact" && "active"}>
          <i className="icon solid fa-envelope" />
          <span>Contact</span>
        </a>
        <a href="https://twitter.com/romanschejbal">
          <i className="icon brands fa-twitter" />
          <span>Twitter</span>
        </a>
      </nav>
      <div id="main">
        {activeSection === "home" ? <Home /> : null}
        {activeSection === "work" ? <Work /> : null}
        {activeSection === "contact" ? <Contact /> : null}
      </div>
    </div>
  );
}
```

Basically a bunch of links, with some icons. The thing I want you to notice is how we conditionally add the `"active"` to the `className` property whenever one of the sections is active.

---

Now here is the main course, change the line:

```javascript
const activeSection = "home";
```

To this:
```javascript
const [activeSection, setActiveSection] = React.useState("home"); // This is the hook, in particular, the useState hook
```

And let's add an `onClick` handler to one of the links. Let's start with the _#work_ link, make it so it looks like the following:
```jsx
<a href="#work" className={activeSection === "work" && "active"} onClick={e => setActiveSection("work")}>
  <i className="icon solid fa-folder" />
  <span>Work</span>
</a>
```

At this point, you should be able to click on the work icon and the app should change its state accordingly.

Now, can you make the other links functional, too? It should be fairly straight forward. Feel free to ask if something isn't clear.

---

## What is this magic?

Do you remember I said something about components being more than functions? And do you remember I mentioned they are scheduled by the React scheduler?

Well, React knows when it's rendering our component and it _swaps_ the `useState` hook for one that's _bound to the component fiber_. What is fiber? Fiber is a structure that represents the component and how it is stored in memory, with its state, effects and whatnot.

Don't worry though, you don't have to know all these details and in fact, many professional React developers don't! That's what's nice about React. In a way, it does so much for us so it's easy and simple to develop complex apps with it. And it doesn't ask us to know all the in and outs of it's working, they can be useful though.

---

Are you done? Congratulations, you've just learned the very basics of React. There are [more hooks](https://reactjs.org/docs/hooks-intro.html) like `useState` that you will eventually need to learn about. But take it easy and hey, it's important to have fun while doing so! __Programming is a hobby.__ :)
