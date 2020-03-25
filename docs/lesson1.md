# Hello and welcome!

Here's what we are going to do today:
1. We will take an existing, static HTML & CSS website
2. Turn it into a React based one and bring it to life a little bit

You can then keep the website, tune it up or whatever.. Maybe even put it into production with a little extra help and make it your portfolio.

We are going to turn this:

![static version](resources/static-page.png)

Into this something more compelling for the eyes like this:

![react version](resources/react-page.png)

Basically instead of one loooong page we'll have menu with icons at the top and a dynamic site that can update its content based on the state of the menu.

---

## Let's get on with it...


First, I need you to understand some subtle differences between HTML & JSX syntax. We've [already said](./intro.md) that JSX is basically _Javascript that let's you write HTML in it_<sub>[1](#notes)</sub>. But because Javascript is a language itself, it has different [reserved words](https://en.wikipedia.org/wiki/Reserved_word) than HTML. So the following code would not compile.

```jsx
<div class="heading">Hello World</div>
```

Can you tell why? Well, the reason why this wouldn't compile is that `class` is one of the reserved words in Javascript, we use it to define a blueprint of an object. Therefore, React introduced a different word that's used to set a class to a DOM element. The `className` property. So if we want to convert the HTML from above into JSX, this is how it would look like:

```jsx
<div className="heading">Hello World</div>
```

With this knowledge, you can now head to the project at [Code Sandbox](#link-goes-here) and start converting parts from the `public/index.html` file into React components. You should think through how you want to divide them, what is the granularity going to be like and so on. But don't worry, you can update and iterate on it while doing so. I have marked the code you need to convert with html comments `<!-- THIS IS THE CODE YOU WANT TO CONVERT INTO REACT -->` and `<!-- DONT REMOVE THE CLOSING BODY & HTML TAGS -->`

### Here's a quick hint for you:

If I were to convert the home section "Jane Doe" with its job description "Senior Astral Projectionist" and photo into a React component, I would just cut and paste the code from `public/index.html` into a new file that I would create at `src/pages/Home.js`, I would also change all properties of `class=` into `className=` and I would end up with the following content:

```jsx
import React from 'react'; // we need to import React whenever we want to use JSX, because <div /> actually compiles to React.createElement('div') thus React needs to be in the scope of our code

// we need to export our function / component so we can import it somewhere else, we export it under the default 
export default function Home() {
  return (
    <article id="home" className="panel intro">
      <header>
        <h1>Jane Doe</h1>
        <p>Senior Stellar Projectionist</p>
      </header>
      <a href="#work" className="jumplink pic">
        <span className="arrow icon solid fa-chevron-right">
          <span>See my work</span>
        </span>
        <img src="..." alt="" />
      </a>
    </article>
  );
}
```

And then I would import and use it inside my `./src/App.js` component like so:

```jsx
import React from "react";
import Home from "./pages/Home"; // this is new

import "./styles/main.scss";

export default function App() {

  return (
    <div id="wrapper">
      <div id="main">
        <Home /> {/* this is also new, also, notice how we write comments inside JSX - like this! */}
      </div>
    </div>
  );
}
```

I hope this make sense, if not, hit me on Slack or anywhere!

Now it's your turn to convert the rest of the sections good luck!

--- 

Once you are done with converting all the sections into respective modules, you should endup with something like this.

_`./src/App.js`_
```jsx
import React from "react";

import Home from "./pages/Home"; 
import Work from "./pages/Work"; 
import Contact from "./pages/Contact";


import "./styles/main.scss";

export default function App() {

  return (
    <div id="wrapper">
      <div id="main">
        <Home /> 
        <Work />
        <Contact />
      </div>
    </div>
  );
}
```

This looks way cleaner than the previous long index.html file, doesn't it?

### One more thing
You will notice, that the site looks exactly the same as it used to, our goal though is to __hide the other sections so only one is active at any time__.

We can, for now hard code it like so:

_`./src/App.js`_
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
      <div id="main">
        {activeSection === "home" ? <Home /> : null}
        {activeSection === "work" ? <Work /> : null}
        {activeSection === "contact" ? <Contact /> : null}
      </div>
    </div>
  );
}
```

Right, now only one section should be active - we initialize the `activeSection` variable to `"home"` and then we ask whether is of a respective value or not and based on that we display the section or not. Very simple so far. It's hardcoded though, in the next lesson we'll see how we can make that value dynamic.

## Notes:
1. remember, these are actually compiled to function calls, i.e. `<div>Hello</div>` converts into `React.createElement('div', null, 'Hello')` so it's not exactly HTML. HTML will be the final output and that's handled by something called React DOM Renderer.