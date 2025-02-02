# React JS

## JSX

- JSX is a syntax extension for JavaScript that looks similar to XML or HTML. It allows developers to write HTML elements and components in a more concise and readable manner within JavaScript files.

- JSX = JavaScript + XML: It combines JavaScript’s power with HTML’s declarative syntax.

```jsx
const element = <h1>Hello, JSX!</h1>;
```

becomes:

```jsx
const element = React.createElement("h1", null, "Hello, JSX!");
```

### Core Concepts

#### Embedding JavaScript in JSX

- Use curly braces {} to embed JavaScript expressions:

```jsx
const name = "Alice";
const element = <h1>Hello, {name}</h1>; // Output: <h1>Hello, Alice</h1>
```

#### JSX Attributes

- Use camelCase for attributes (e.g., className instead of class, onClick instead of onclick).

- Inline styles are passed as objects:

```jsx
const style = { color: "red", fontSize: "20px" };
<div style={style}>Styled Text</div>;
```

#### JSX Children

- JSX can nest elements, but must have a single root element:

```jsx
// Valid
const element = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// Invalid (no parent wrapper)
const invalidElement = (
  <h1>Title</h1>
  <p>Content</p>
);
```

#### Self-Closing Tags

- Tags with no children must close with />:

```jsx
<img src="image.jpg" alt="Example" /> // ✅ Correct
<br>                                 // ❌ Incorrect (needs self-closing)
```

#### Components in JSX

- Custom components start with a capital letter (to differentiate from HTML tags):

```jsx
function MyComponent() {
  return <div>Hello, World!</div>;
}
usage: <MyComponent />;
```

#### JSX Under the Hood

- JSX compiles to React.createElement() calls:

```jsx
const element = <h1>Hello, JSX!</h1>;
// Compiles to:
const element = React.createElement("h1", null, "Hello, JSX!");
```

#### BEST PRACTICES

- Multi-line JSX: Wrap in parentheses () for readability:

```jsx
const element = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);
```

- Extract Complex Logic: Keep JSX clean by moving logic outside:

```jsx
const isLoggedIn = true;
const greeting = isLoggedIn ? <Welcome /> : <Login />;
return <div>{greeting}</div>;
```

## PROPS

- Props are a way of passing data from a parent component to a child component in React. They are immutable and provide a way of making components dynamic and reusable

- Props are short for properties, and they are used to pass data from a parent component to a child component. This mechanism enables components to be reusable and adaptable, as they can receive different sets of props to customize their behavior and appearance. Props are essentially JavaScript objects containing key-value pairs, where the keys represent the prop names and the values contain the corresponding data.

```jsx
import React from "react";

const PassingProps = (props: { message: string }) => {
  return <div>{props?.message}</div>;
};

export default PassingProps;
```

### Passing Props to Components

-Props are passed into a component in JSX as attributes, with prop names matching what is defined in the component.

```jsx
<ContactDetails name="Fred" email="fred@somewhere.com" />
```

-We can also pass multiple props to a component, and React allows passing arrays and objects as prop values.

```jsx
const productData = { title: "A book", price: 29.99, id: "p1" };
<Product data={productData} />;
```

### Accessing Props

-Props can be accessed using dot notation or destructuring, making the code cleaner and more readable.

- For example, if we have a MyComponent component that receives a title and description prop, we can access them like this:

```jsx
const MyComponent = ({ title, description }) => {
  return (
    <div>
      <h1>{title}</h1>
      <p>{description}</p>
    </div>
  );
};
```

> Class components can access props using this.props.propName.

```jsx
class User extends React.Component {
  render() {
    return <h1>{this.props.name}</h1>;
  }
}
```

#### Default Props

- Default props can be defined using the defaultProps property.
- Provide fallback values if a prop isn’t passed:

```jsx
// Functional Component
function User({ name = "Guest" }) {
  return <h1>Hello, {name}</h1>;
}

// Class Component (using static defaultProps)
class User extends React.Component {
  static defaultProps = { name: "Guest" };
  render() {
    return <h1>{this.props.name}</h1>;
  }
}
```

#### Children Prop

- Children prop allows passing content between opening and closing tags of a component
- Access nested content via props.children:

```jsx
// Parent
function Card() {
  return (
    <div className="card">
      <CardBody>This is the card content</CardBody>
    </div>
  );
}

// Child (CardBody)
function CardBody({ children }) {
  return <div className="card-body">{children}</div>;
}
```

## STATE

- State in React refers to the internal data that a component can hold and manage. It allows components to store and update data that can be used to render dynamic content.

- Mutable data that determines a component’s behavior and rendering. It’s private to the component and triggers re-renders when updated.

### Why It Matters

-Enables interactive UIs (forms, animations, dynamic content).
-Reflects real-time changes (e.g., API responses, user input).
-Drives component logic (e.g., loading states, error handling).

### State vs Props

| State                                      | Props                       |
| ------------------------------------------ | --------------------------- |
| Managed internally by the component        | Passed from parent to child |
| Mutable (updated via setState or useState) | Immutable (read-only)       |

```jsx
import React, { useState } from "react";
import { useState } from "react";
const State = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
};
export default State;
```

### Core-Concepts

## LIFECYCLE METHODS

In React, lifecycle methods are special methods in class components that allow you to run code at particular times during a component’s life cycle. The lifecycle consists of three phases:

1.Mounting (when a component is added to the DOM)
2.Updating (when a component is re-rendered)
3.Unmounting (when a component is removed from the DOM)

### Mounting Phase

-This phase is triggered when a component is created and inserted into the DOM.

> Lifecycle Methods in Mounting:

1.constructor()
• Called before the component is mounted.
• Used for initializing state and binding methods.
• Avoid side effects (e.g., fetching data) here.

```jsx
constructor(props) {
  super(props);
  this.state = { counter: 0 };
}
```

2.static getDerivedStateFromProps(props, state)
• Used to update the state based on props.
• Runs before rendering.
• Returns an object to update state or null to do nothing.

```jsx
static getDerivedStateFromProps(props, state) {
  if (props.initialValue !== state.counter) {
    return { counter: props.initialValue };
  }
  return null;
}
```

3.render()
• The only required method in a class component.
• Returns the JSX to render the component.

```jsx
render() {
  return <h1>Counter: {this.state.counter}</h1>;
}
```

4.componentDidMount()
• Invoked immediately after the component is mounted.
• Best place for side effects like fetching data or setting up subscriptions.

```componentDidMount() {
  console.log('Component mounted');
}
```

### Updating Phase

Triggered when the component’s state or props change, causing a re-render.

> Lifecycle Methods in Updating:

-static getDerivedStateFromProps(props, state)
•Runs on both mount and update to sync state with props if needed.

-shouldComponentUpdate(nextProps, nextState)
• Used to control whether the component should re-render.
• Returns true by default. Returning false skips render() and subsequent lifecycle methods.

```jsx
shouldComponentUpdate(nextProps, nextState) {
 return nextProps.counter !== this.props.counter;
```

-render()
• Called to update the component’s UI.

-getSnapshotBeforeUpdate(prevProps, prevState)

• Captures information (like scroll position) before the DOM is updated.
• Returns a value that is passed to componentDidUpdate.

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
  if (prevState.counter !== this.state.counter) {
    return `Counter changed from ${prevState.counter} to ${this.state.counter}`;
  }
  return null;
}
```

-componentDidUpdate(prevProps, prevState, snapshot)

• Invoked immediately after an update occurs.
• Useful for performing side effects after a re-render.

```jsx
componentDidUpdate(prevProps, prevState, snapshot) {
  if (snapshot) {
    console.log(snapshot);
    }
}
```

### Unmounting Phase

Triggered when the component is removed from the DOM.

> Lifecycle Methods in Unmounting:

1.componentWillUnmount()  
• Called just before the component is unmounted and destroyed.
• Use it for cleanup (e.g., removing event listeners or canceling network requests).

```jsx
componentWillUnmount() {
    console.log('Component will unmount');
}
```

## ERROR BOUNDARY

An Error Boundary is a React component that catches JavaScript errors in its child component tree during rendering, lifecycle methods, and in constructors of child components. It allows developers to gracefully handle errors by displaying a fallback UI instead of crashing the entire application

### When Do Error Boundaries Catch Errors?

- In lifecycle methods
- In constructors of child components
- During rendering

### What Error Boundaries Cannot Catch?

- Errors inside an event handler
- Errors in asynchronous code (e.g., setTimeout or fetch)
- Errors from the server
- Errors in the Error Boundary itself

### Creating an Error boundary

- An Error Boundary must be a class component.

```jsx
import { Component } from "react";

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = {
      hasError: false,
      errorMessage: "",
    };
  }

  static getDerivedStaeFromError(error) {
    return { hasError: true, message: error };
  }

  componentDidCatch(error, errorInfo) {
    console.log("ErrorBoundary caught an error", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>{this.state.errorMessage}</h1>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

### Using error boundary

- Wrap any part of your application where you want to catch errors:

```jsx
import ErrorBoundary from "./1-creating-error-boundary";
import Dummy from "./dummy-component";

const UsingErrorBoundary = () => {
  return (
    <ErrorBoundary>
      <Dummy />
    </ErrorBoundary>
  );
};
export default UsingErrorBoundary;
```

### Handling Error in Functional component

```jsx
import ErrorBoundary from "./1.creating-error-boundary";

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <p>Something went wrong:</p>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function BUggyComponen() {
  throw new Error("Crashed");
}

export default function UsingErrorBoundary() {
  return (
    <ErrorBoundary FallbackComponent={ErrorFallback}>
      <BUggyComponen />
    </ErrorBoundary>
  );
}
```

## EVENT HANDLING

React uses camelCase to handle events. Functions can be defined to handle events such as clicks, changes, etc., providing interactivity to the components.

```jsx
const EventHandlingBasics = () => {
  const handleClick = () => {
    console.log("Clicked");
  };

  return <button onClick={handleClick}>Click Me </button>;
};
```

-React listens for all events at the top level using a single event listener, which reduces the memory footprint and initial setup time for event handlers. This approach, known as event delegation, simplifies event handling by centralizing the event logic at the root of the component tree. When an event occurs in a child component, React captures the event at the root of the component tree and then traverses down to the specific component that triggered the event.

### Event Propagation

Event propagation in React is based on the component hierarchy rather than the DOM hierarchy. This means that when an event occurs in a child component, React will traverse down to the specific component that triggered the event, rather than bubbling up through the DOM tree. This approach allows for more efficient event handling and reduces the number of event listeners attached to individual DOM elements.

#### BEST PRACTICE

-When handling events in React, it's essential to follow best practices, such as using the preventDefault() method to prevent the browser's default behavior, and using the onSubmit prop to handle form submissions.

> Passing Arguments to Event Handlers

```jsx
function handleClick(item) {
  console.log("Clicked:", item);
}

<button onClick={() => handleClick(specificItem)}>Click me</button>;
```

### Controlled Components

-Controlled components in React have inputs and their state controlled by React. They receive their current value and the onChange handler as props, making them controlled by React and not by the DOM

```jsx
import React, { useState } from "react";

const Controlled = () => {
  const [inutValue, setInputValue] = useState("");

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };
  return (
    <input
      type="text"
      value={inputValue}
      onChange={hadleInputChange}
      placeholder="type text"
    />
  );
};

export default Controlled;
```

## HIGH ORDER COMPONENT

-Higher Order Components (HOCs) are functions that take a component and return a new component with additional functionality. They are a way of reusing the component's logic.

```jsx
import React from "react";

const WithLogger = (wrappedComponent) => {
  return class extends React.Component {
    render() {
      return (
        <div>
          <WrappedComponent {...this.props} />
        </div>
      );
    }
  };
};

export default WithLogger;
```

-using with logger Hoc

```jsx
import React from "react";
import WithLogger from "./HighOrderComponent";

const UseCaseHOC = () => {
  return <div>UseCaseHOC</div>;
};

const enhancedComponent = WithLogger(UseCaseHOC);
export default enhancedComponent;
```

## RENDERING LISTS

-React provides the map function to render lists of items dynamically. Each item in the array is mapped to a React element, making it easier to render dynamic content.

````jsx
import React from 'react'

 const RenderingList = () => {
     const items = ['Item1','Item2','Item3'];
   return (
     <ul>
         {items.map((item,index) => (
             <li key={index}>{item}</li>
         ))}
     </ul>
   )
 }

 export default RenderingList```
````

## CONTEXT API

-The Context API in React offers a way of transmitting data through the component tree without having to pass props manually at each level. It's useful for sharing values such as themes or authentication status.

> creating context

```import React, { createContext } from "react";

const ThemeContext = createContext("light");

export default ThemeContext;
```

> Using context

```jsx
import { useContext } from "react";
import ThemeContext from "./ThemeContext";

const ThemeComponent = () => {
  const theme = useContext(ThemeContext);
  return (
    <div style={{ backgroundColor: theme }}>
      <h1>Theme Component</h1>
      <p>Current Theme: {theme}</p>
    </div>
  );
};
export default ThemeComponent;
```

## KEYS IN REACT

-Keys in React help identify which items have been changed, added or removed. They should be unique within the list and help React with efficient updates.

```jsx
const KeysExample = () => {
  const data = [
    { id: 1, name: "Item 1" },
    { id: 2, name: "Item 2" },
    { id: 3, name: "Item 3" },
  ];

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
};

export default KeysExample;
```

## FORMS IN REACT

-Handling forms in React involves managing form data using state and handling form submission via event handlers. Controlled components are used to synchronize form elements with React's state.

```jsx
import { useState } from "react";
const Forms = () => {
  const [formData, setFormData] = useState({
    username: "",
    password: "",
  });

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  return (
    <form>
      <label>
        {" "}
        UserName:
        <input
          type="text"
          placeholder="Enter your name"
          value={formData.username}
          onChange={handleChange}
        />
      </label>
      <label>
        Password:
        <input
          type="password"
          placeholder="Enter your password"
          value={formData.password}
          onChange={handleChange}
        />
      </label>
      <button type="submit">Submit</button>
    </form>
  );
};

export default Forms;
```

## RENDER PROPS

-Render Props is a technique for sharing code between React components using a prop whose value is a function. This allows for the dynamic composition of components

```jsx
import React, { useState } from "react";

const MouseTracker = () => {
  const [position, setPosition] = useState({
    x: 0,
    y: 0,
  });

  const handleMouseMove = (event) => {
    setPosition({
      x: event.clientX,
      y: event.clientY,
    });
  };
  return (
    <div onMouseMove={handleMouseMove}>
      <h1>Mouse Tracker</h1>
      {render(position)}
    </div>
  );
};

export default MouseTracker;
```

```jsx
import React from "react";
import MouseTracker from "./MouseTracker";

const RenderPropsExample = ({ render }) => {
  return (
    <MouseTracker
      render={(position) => {
        <p>
          MouseTracker: {position.x} ,{position.y}
        </p>;
      }}
    />
  );
};

export default RenderPropsExample;
```

## CSS MODULES

-CSS Modules help define the scope of styles for a specific component, avoiding global style conflicts. Each component can have its own CSS module with locally scoped styles.

```.myComponent {
  color: green;
}
```

```jsx
import React from "react";
import styles from "./CSSModulesExample.module.css";

const CSSModulesExample = () => {
  return <p className={styles.myComponent}>Styled with CSS Modules</p>;
};

export default CSSModulesExample;
```

## HOOKS

React Hooks are functions that allow functional components to manage state and side effects. They were introduced in React 16.8 and provide a more concise way of working with state and lifecycle methods in functional components.
}

```jsx
import React, { useState } from "react";

export const UseStatDummy = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>}
    </div>
  );
};
```
