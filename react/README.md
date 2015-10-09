# Airbnb React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)

## Basic Rules

  - Only include one React component per file.
  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.

## Class vs React.createClass

  - Use class extends React.Component unless you have a very good reason to use mixins.

    ```jsx
    // bad
    const Listing = React.createClass({
    render() {
      return <div />;
    }
    });
    
    // good
    class Listing extends React.Component {
    render() {
      return <div />;
    }
    }
    ```

## Naming

  - **Extensions**: Use `.jsx` extension for React components.
  - **Filename**: Use Kebab Case for filenames. E.g., `reservation-card.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances:
    ```javascript
    // bad
    const reservationCard = require('./reservation-card.jsx');

    // good
    const ReservationCard = require('./reservation-card.jsx');

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

    **Component Naming**: Use the filename as the component name. For example, `reservation-card.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:
    ```javascript
    // bad
    const Footer = require('./footer/footer.jsx')

    // bad
    const Footer = require('./footer/index.jsx')

    // good
    const Footer = require('./footer.jsx')
    ```

    ### Event handlers
    
    ##### Name handlers `handleEventName`.
    
    Example:
    
    ```jsx
    <Component onClick={this.handleClick} onLaunchMissiles={this.handleLaunchMissiles} />
    ```
    
    ##### Name handlers in props `onEventName`.
    
    This is consistent with React's event naming: `onClick`, `onDrag`,
    `onChange`, etc.
    
    Example:
    
    ```jsx
    <Component onLaunchMissiles={this.handleLaunchMissiles} />
    ```

## Declaration
  - Do not use displayName for naming components. Instead, name the component by reference.

    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment
  - Follow these alignment styles for JSX syntax

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Spazz />
    </Foo>
    ```

## Quotes
  - Always use double quotes (`"`) for JSX attributes, but single quotes for all other JS.

    ```javascript
    // bad
    <Foo bar='bar' />
    
    // good
    <Foo bar="bar" />
    
    // bad
    <Foo style={{ left: "20px" }} />
    
    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing
  - Always include a single space in your self-closing tag.
    ```javascript
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## Props
  - Always use camelCase for prop names.
    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

## Parentheses
  - Wrap JSX tags in parentheses when they span more than one line:
    ```javascript
    /// bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags
  - Always self-close tags that have no children.
    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line.
    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods
  - Use underscore prefix for "private" methods of a React component.
    ```javascript
    // bad
    React.createClass({
      _somePrivateMethod() {
        // do stuff
      }

      // other stuff
    });

    // good
    class extends React.Component {
      somePrivateMethod() {
        // do stuff
      }

      // other stuff
    });
    ```

## Ordering

  - Ordering for class extends React.Component:
  
  1. constructor
  1. optional static methods
  1. getChildContext
  1. componentWillMount
  1. componentDidMount
  1. componentWillReceiveProps
  1. shouldComponentUpdate
  1. componentWillUpdate
  1. componentDidUpdate
  1. componentWillUnmount
  1. *clickHandlers or eventHandlers* like handleSubmit() or handleDescriptionChange()
  1. *getter methods for render* like getSelectReason() or getFooterContent()
  1. *Optional render methods* like renderNavigation() or renderProfilePicture()
  1. render

  - How to define propTypes, defaultProps, contextTypes, etc...  

      ```javascript
      import React, { Component, PropTypes } from 'react';
      
      const propTypes = {
        id: PropTypes.number.isRequired,
        url: PropTypes.string.isRequired,
        text: PropTypes.string,
      };
      
      const defaultProps = {
        text: 'Hello World',
      };
      
      export default class Link extends Component {
        static methodsAreOk() {
          return true;
        }
      
        render() {
          return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
        }
      }
      
      Link.propTypes = propTypes;
      Link.defaultProps = defaultProps;
      ```

  - Ordering for React.createClass:

  1. displayName
  1. propTypes
  1. contextTypes
  1. childContextTypes
  1. mixins
  1. statics
  1. defaultProps
  1. getDefaultProps
  1. getInitialState
  1. getChildContext
  1. componentWillMount
  1. componentDidMount
  1. componentWillReceiveProps
  1. shouldComponentUpdate
  1. componentWillUpdate
  1. componentDidUpdate
  1. componentWillUnmount
  1. *clickHandlers or eventHandlers* like handleSubmit() or handleDescriptionChange()
  1. *getter methods for render* like getSelectReason() or getFooterContent()
  1. *Optional render methods* like renderNavigation() or renderProfilePicture()
  1. render

#### Use [propTypes](http://facebook.github.io/react/docs/reusable-components.html).

React Components should always have complete propTypes.  Every
attribute of this.props should have a corresponding entry in
propTypes.  This documents that props need to be passed to a model.
([example](https://github.com/Khan/webapp/blob/32aa862769d4e93c477dc0ee0388816056252c4a/javascript/search-package/search-results-list.jsx#L14))

If you as passing data through to a child component, you can use
the prop-type `<child-class>.propTypes.<prop-name>`.

Avoid these non-descriptive prop-types:
   * `React.PropTypes.any`
   * `React.PropTypes.array`
   * `React.PropTypes.object`

Instead, use 
   * `React.PropTypes.arrayOf`
   * `React.PropTypes.objectOf`
   * `React.PropTypes.instanceOf`
   * `React.PropTypes.shape`

As an exception, if passing data through to a child component, and you
can't use `<child-class>.propTypes.<prop-name>` for some reason, you
can use `React.PropType.any`.

#### *Never* store state in the DOM.

Do not use `data-` attributes or classes.  All information
should be stored in Javascript, either in the React component itself,
or in a React store if using a framework such as Redux.

----------------------------------
### React libraries and components

#### Minimize use of jQuery.

*Never* use jQuery for DOM manipulation.

Try to avoid using jQuery plugins.  When necessary, wrap the jQuery
plugin with a React component so you only have to touch the jQuery
once.

**[⬆ back to top](#table-of-contents)**
