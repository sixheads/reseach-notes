# Hooks Intro - The Modern React Bootcamp

- [Source](https://www.udemy.com/course/modern-react-bootcamp)

## Our simple counter hook

```javascript
import React, { useState } from "react";

function CounterHooks() {
  // ----- 1 ------ 2 -------------- 3
  const [count, setCount] = useState(0);
  // 1. returns a piece of state - count
  // 2. returns a function to update the piece of state - setCount
  // 3. sets the initial value of our piece of state

  return (
    <div>
      {/* ------ current count value */}
      <h2>The Count Is: {count}</h2>
      {/* onClick function that uses setCount function to update count */}
      <button onClick={() => setCount(count + 1)}>Add 1</button>
    </div>
  );
}

export default CounterHooks;
```

## Creating a Toggle hook

```javascript
// 1. import useState
import { useState } from "react";

// 2. create our function that takes an initial value which we're defaulting to false
function useToggle(initialVal = false) {
  // 3. setup our useState with the initial value set for our state
  const [toggled, setToggled] = useState(initialVal);
  // 4. our function simply does the opposite of whatever state is set too
  const toggle = () => {
    setToggled(!toggled);
  };
  // 5. we return our state and our toggle function rather than just setToggled
  return [toggled, toggle];
}

export default useToggle;
```

- then we use our new toggler hook in our component

```javascript
import React from "react";
// 1. bring in our useToggle hook rather than useState above
import useToggle from "./hooks/useToggle";

function TogglerWithHook() {
  // 2. two examples using our hook
  // 3. we assign different variable for each but both pull from useToggle
  const [isHappy, toggleIsHappy] = useToggle(true);
  const [isHeartbroken, toggleIsHeartbroken] = useToggle(false);

  return (
    <div>
      {/* here we are calling our toggled function from the hook but as toggleIsHappy */}
      <h1 onClick={toggleIsHappy}>{isHappy ? "😃" : "😢"}</h1>
      <h1 onClick={toggleIsHeartbroken}>{isHeartbroken ? "❤️" : "💔"}</h1>
    </div>
  );
}

export default TogglerWithHook;
```

## Simple form setup

```javascript
import React, { useState } from "react";

const SimpleFormHooks = () => {
  const [email, setEmail] = useState("");
  const handleChange = (e) => {
    setEmail(e.target.value);
  };
  return (
    <div>
      <h1>This value is {email}</h1>
      <input type="text" value={email} onChange={handleChange} />
    </div>
  );
};

export default SimpleFormHooks;
```

## Creating our useInputState hook

- first we setup our hook

```javascript
import { useState } from "react";

// our hook takes an initial value
export default (initialVal) => {
  // our initial value is set as state
  const [value, setValue] = useState(initialVal);
  // we setup our handleChange function
  const handleChange = (e) => {
    setValue(e.target.value);
  };

  // and our reset function
  const reset = () => {
    setValue("");
  };

  // then return the value and both functions
  return [value, handleChange, reset];
};

// To use it now we pull our the returned values and rename them to suit our use case
const [email, updateEmail, resetEmail] = useInputState("");
const [password, updatePassword, resetPassword] = useInputState("");
```

- then to use our hook:

```javascript
import React from "react";
import useInputState from "./hooks/useInputState";

const SimpleFormInputHook = () => {
  const [email, updateEmail, resetEmail] = useInputState("");
  const [password, updatePassword, resetPassword] = useInputState("");
  return (
    <div>
      <h1>
        Email: {email} & Password: {password}
      </h1>
      <input type="text" value={email} onChange={updateEmail} />
      <input type="text" value={password} onChange={updatePassword} />
      <button onClick={resetEmail}>Reset Email</button>
      <button onClick={resetPassword}>Reset Password</button>
    </div>
  );
};

export default SimpleFormInputHook;
```

## useEffect

- with use effect we can trigger something after every render

```javascript
// 1. import useEffect
import React, { useState, useEffect } from "react";

const Clicker = () => {
  const [count, setCount] = useState(0);
  // 2. useEffect takes a function
  useEffect(() => {
    // here we just write a new doc title each render
    document.title = `You clicked ${count} times`;
  });
  return <button onClick={() => setCount(count + 1)}>Click me {count}</button>;
};

export default Clicker;
```

## useEffect calling an API

- we can useEffect when fetching data from an api
- we can also set it to only run when a single thing changes not every rerender

```javascript
import React, { useState, useEffect } from "react";
import axios from "axios";

const SWMovies = () => {
  // 1. set state for the number chosen in the select form below
  const [number, setNumber] = useState(1);
  // 2. create state for the Movie chosen - empty string at first
  const [movie, setMovie] = useState("");

  // 3. Create our useEffect
  useEffect(() => {
    // 4. use axios and run an async function
    // needs to be an addition function inside useEffect
    async function getData() {
      const response = await axios.get(
        `https://swapi.dev/api/films/${number}/`
      );
      // console.log(response)
      // 5. set the movie to be the response data
      setMovie(response.data);
    }
    // returns getData
    getData();
  }, [number]); // using number here means it only runs when number changes rather than every render
  return (
    <div>
      {/* Now we use the data returned */}
      <h1>{movie.title}</h1>
      <p>{movie.opening_crawl}</p>
      <select value={number} onChange={(e) => setNumber(e.target.value)}>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
      </select>
    </div>
  );
};

export default SWMovies;
```
