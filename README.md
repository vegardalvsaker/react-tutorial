# react-tutorial
__DO NOT COPY-PASTE ALL THE CODE EXAMPLES. TYPE THEM IN MANUALLY IN ORDER TO GET THE FEEL OF WRITING REACT CODE INTO YOUR FINGERS__

Make sure you've installed Node.js (and peferably Visual Studio Code).

Open a terminal and write

```console 
npx create-react-app tutorialapp --typescript
```

The preceding command will create a react project in the folder ```tutorialapp```. This will download all the files required for a react project inside of the folder `tutorialapp/node_modules` and set up a few basic files to get you started. When this is finished, run ```cd tutorialapp``` and run ```npm start```.
This will start the development server and render the changes in the browser as you save the files. 
Open the folder ```tutorialapp``` in VS Code, open up ```App.tsx```  and change some of the text in white (line 12 and/or 20), save the file and see the changes in the browser at ```http://localhost:3000```.

# Writing your own functional component

Delete the existing component (lines 6-24) and write your own component named ```App``` which returns a `<h1>` with the text 'My first React component'.
You should now have the following component in `App.tsx`:

```javascript
const YourComponentName: React.FC = () => {
  return (
    <h1>My first React component</h1>
  );
}
```

## Rendering several JSX-elements (html-tags)
_When rendering more than one element, you need to render them inside a `<div>`-tag. A React component's return statement may not have several JSX-elements (HTML) in the same level.
For example, this is __not__ valid:_
```javascript
return(
  <h3>Vegard</h3>
  <h3>Alvsaker</h3>
  <h3>vegi@gmail.com</h3>
);
```

but this is:
```javascript
return(
  <div>
    <h3>Vegard</h3>
    <h3>Alvsaker</h3>
    <h3>vegi@gmail.com</h3>
  </div>
);
```

# Rendering a component from App
Below the `App`-component, create a new component which returns three `<h3>`-tags, where you insert a first name, a surname and an email. Hardcode these values for now.

Your `App.tsx` should now look something like this:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import logo from './logo.svg';
import './App.css';

const App: React.FC = () => {
  return (
    <h1>My first React component</h1>
  );
}

const Person: React.FC = () => {
  return (
    <div>
      <h3>Jens</h3>
      <h3>August</h3>
      <h3>jensaugust@hotelcæsar.no</h3>
    </div>
  )
}

export default App;
ReactDOM.render(<App />, document.getElementById('root'));
```
If you have saved your file, you will notice that were no changes in the browser. This is because `App` is the only component 
which is rendered. Therefore, we will need to render `Person` from `App`'s return statement. Add as many `<Person/>`s as you want

`App` should now look something like this:

```javascript
const App: React.FC = () => {
  return (
    <div>
      <Person />
      <Person />
      <Person />
    </div>
  );
}
```
# Declaring a component's props

In order to avoid hard coding the name and email inside of the `Person`-component, we will need to pass this information through something called props. This will make the `Person`-component more reusable.

Before we can pass props to `Person`, we need to define which props the component can receive. This is a TypeScript specific feature.
Above the `Person`-component, add this code:

```javascript
type PersonProps = {
  firstName: string,
  lastName: string,
  email: string
}
```

Now change the signature of `Person` from this:
```javascript
const Person: React.FC = () => {
```

to this: 
```javascript
const Person: React.FC<PersonProps> = props => {
```
This will tell the `Person`-component to use the PersonProps. The props is available by accessing the props parameter we just added. 


_The reason for replacing the parenthesis with `props`, instead of putting them inside the parenthesis `(props)` is because you don't need them when there is only one parameter. You don't have to think too much about this_

You will now have a red line under the `Person`-components, this is because we are not passing the required props. We will fix this after the next step.

Inside the return statement of `Person`, replace the hardcoded values with "First name: {props.firstName}" etc. In order to access an object's value inside of the return statement, you will need to wrap it in curly braces. To test this, try to write `props.firstname` without the curly braces

Your `Person`-component should now look like this:
```javascript
const Person: React.FC<PersonProps> = props => {
  return (
    <div>
      <h3>First name: {props.firstName}</h3>
      <h3>Last name: {props.lastName}</h3>
      <h3>email: {props.email}</h3>
    </div>
  );
}

```

# Passing props to a component
Hover over one of the `<Person />`-components with a red line under and read the text. It tells us that it is missing the properties `firstName`, `lastName` and `email`, so let's add them. Do the following and add unique values to all three `<Person/>`s:
```javascript
<Person
        firstName="Jens August"
        lastName="Olsen"
        email="jens@cæsar.no" />
```

The `App`-component should now look something like this:
```javascript
const App: React.FC = () => {
  return (
    <div>
      <hr />
      <Person
        firstName="Jens August"
        lastName="Olsen"
        email="jens@cæsar.no" />
      <hr />
      <Person
        firstName="Ole"
        lastName="Brum"
        email="ole@brum.no" />
      <hr />
      <Person
        firstName="Kaptein"
        lastName="Sabeltann"
        email="captain@kjuttaviga.no" />
      <hr />
    </div>
  );
}
```
The `<hr />`-tags adds a dividing line between each Person.

# Reduce duplication and make it dynamic

To avoid repeating ourselves, we will create an array of persons that we will render, instead of explicitly typing a `<Person />` for every person we have.

Start by adding this array of `PersonProps` above the `App`-component:
```javascript
const persons: PersonProps[] = [
  {
    firstName: "Jens August",
    lastName: "Olsen",
    email: "jens@cæsar.no"
  },
  {
    firstName: "Ole",
    lastName: "Brum",
    email: "ole@brum.no"
  },
  {
    firstName: "Kaptein",
    lastName: "Sabeltann",
    email: "captain@kjuttaviga.no"
  }
];
```
Now, remove everything from the return statement of `App` except for the `<div>` and `</div>`. We will now use a built in JavaScript-function for Arrays: map(). We will use this function as a loop for our array `persons` to dynamically display as many persons there are in the array. Add this code inside of the `<div>`-tags:
```javascript
{persons.map(p => (
        <div key={p.email}>
          <hr />
          <Person
            firstName={p.firstName}
            lastName={p.lastName}
            email={p.email}
          />
        </div>
      ))}
```

# Final version
You `App.tsx` should now look like this:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import logo from './logo.svg';
import './App.css';

const persons: PersonProps[] = [
  {
    firstName: "Jens August",
    lastName: "Olsen",
    email: "jens@cæsar.no"
  },
  {
    firstName: "Ole",
    lastName: "Brum",
    email: "ole@brum.no"
  },
  {
    firstName: "Kaptein",
    lastName: "Sabeltann",
    email: "captain@kjuttaviga.no"
  },
];

const App: React.FC = () => {
  return (
    <div>
      {persons.map(p => (
        <div key={p.email}>
          <hr />
          <Person
            firstName={p.firstName}
            lastName={p.lastName}
            email={p.email}
          />
        </div>
      )
      )}
    </div>
  );
}

type PersonProps = {
  firstName: string,
  lastName: string,
  email: string
}

const Person: React.FC<PersonProps> = props => {
  return (
    <div>
      <h3>First name: {props.firstName}</h3>
      <h3>Last name: {props.lastName}</h3>
      <h3>email: {props.email}</h3>
    </div>
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('root'));
```
