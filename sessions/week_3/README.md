# CSS and Themes

:::warning

## Session Dependencies

This practical assumes you are up-to-date with the homework from last week. 
[If, for whatever reason you have fallen behind, you can get the latest solution from my github repository](joeappleton18/web-dev-industry-practical/) 

[You will also need to access the mockup for these practical sessions](https://www.figma.com/file/rTbqRpRWOw7UYg28SBcxQv/web-dev-pratical-task-made-using-toxin-ui?node-id=31264%3A79)

:::


:::warning

### Essential Reading :closed_book:


[The React documentation on styles](https://reactjs.org/docs/faq-styling.html)

[React: CSS in JS techniques comparison](https://github.com/MicheleBertoli/css-in-js)


[The basics of styled components](https://styled-components.com/docs/basics)


:::




This week is going to be particularly interesting,  I am going to present a solution that solves a fundamental challenge application developers face.  The challenge can be reduced to the following question:

How can we ensure a consistent look and feel across our applications?

## A Style Guide

There are, of course, several ways to address the above question. To start with, a simple style guide can go a long way in presenting a solution. 

![](./assets/style_guide.jpg)

>> [Figure 1, the style guide for our tracking application](https://www.figma.com/file/rTbqRpRWOw7UYg28SBcxQv/web-dev-pratical-task-made-using-toxin-ui?node-id=31275%3A269)


The above style guide, [part of our overall application design](https://www.figma.com/file/rTbqRpRWOw7UYg28SBcxQv/web-dev-pratical-task-made-using-toxin-ui?node-id=31275%3A269), lays the foundation colour schemes and font sizings. Crucially, it is in a language developers can quickly understand.  

:::warning 
For assessment 1 we will not need to create a style guide. We are focused on a solution, not granular design choices.  For assessment two, Figure 1 represents the minimum level of detail I expect. This is fine if you are taking the, part 2, development route. However, if you are pursuing design, you'll need to take things further and consider aspects such as logo, branding and spacing. 
:::

## A Style Guide to A Living Themes

We need a way of converting our style guide, above, into some sort of theming solution. We can, of course, use CSS, however, it somewhat lacks sophistication. Pre-processors, such as Syntactically Awesome Style Sheets (SASS), addressed some of the limitations of CSS by extending its capabilities allowing control structures and variables to be used. 

SASS solved a lot of problems and still should be considered a go-to-solution for simple sites that require little to no JavaScript functionality. However, in this course, we are working with single-page web applications and we must assume ourselves and any developer on the project will be well versed in JavaScript. As such, while we could use SASS, JavaScript is far more powerful and can achieve everything SASS can and more. In realising this idea, recently we have seen a rise in `CSS-in-JS` libraries.  Combining CSS with our JS is an odd concept, to begin with.  However, unlike SASS, there is no need to learn a different syntax - you already know JavaScript.  In summary, `CSS-in-JS` presents a very shallow learning curve and the upside of a powerful extension to traditional CSS. 

## CSS-in-JS

You have already used CSS-in-JS. For instance, consider setting the inline style attribute equal to some object:

```js
    const innerBar = {
        background:
        ...
        height: `${percentage}%`,
        ...
    };

    return (
        <div style={barStyle}>
            <div style={innerBar}></div>
        </div>
    );

```

Creating inline styles using JavaScript objects, is one way of working using a CSS-in-JS pattern, and a perfectly reasonable one. However, many third-party libraries provide more sophisticated solutions (see https://github.com/MicheleBertoli/css-in-js).

While, as mentioned above, there are many styling solutions. [https://styled-components.com/](https://styled-components.com/docs/basics#motivation), for me, presented itself as a simple yet powerful theming solution for React Applications. 

:::tip 
## Task 1  - Understand the Why
I always like to know the philosophical position of any third-party library that I use. To access this, I consider what sort of problem they are trying to solve? Moreover, I ask myself the question; will my application, moving forward run into this problem? 

For this first task, spend [5 minutes reading the motivations behind](https://styled-components.com/docs/basics#motivation) styled-components. 

:::

### Working with styled-components 

Let's get going, the first thing we need to do is install the styled-components node package.  This is a little more involved than I would like as, to get the most out of the styled-components library, we need to install and configure the babel preset.

:::tip

## Task 2  -  Installing styled-components 


From command line use npm to install the following dependencies:

```terminal 
	npm install --save-dev babel-plugin-styled-components
	npm install --save styled-components
```  

Next, need to tell babel to actually use the `babel-plugin-styled-components` preset component.  In the root of your project, create a `.babelrc` file and add the following snippet of JSON:

```javascript
	{
 		"plugins": ["babel-plugin-styled-components"]
	}
```

Remember, we used create-react-app to scaffold our application. As such, under the hood, all of our configuration is taken care of. However this presents an issue as the default config will ignore our `.babel` file. We can "eject" the application, this will move all of the configuration files directly into our project this seems quite extreme. Instead, we are going to use [customize-cra](https://github.com/arackaf/customize-cra) to edit our config without ejecting.   

First, install `customize-cra` and its dependency `react-app-rewired`.

```terminal
	npm install --save-dev react-app-rewired customize-cra
```

Next, create a `config-overrides.js` file in your root directory and add the following code snippet

```js
const { useBabelRc, override} = require('customize-cra')

module.exports = override(
 useBabelRc()
);
```

Finally, updated your `package.json` file to run `react-app-rewired` as opposed to `react-scripts`. The scripts section of your `package.json` file  should read as follows:

```javascript
"scripts": {
 "start": "react-app-rewired start start",
 "build": "react-app-rewired start build",
 "test": "react-app-rewired start test",
 "eject": "react-scripts eject"
 }
```
:::

## Styled-Components - the basics

Styled components componetise your styles. In their words, "[it removes the mapping between components and styles](https://styled-components.com/docs/basics#getting-started)". Like React in general, this is going to be an odd concept begin with. 

Let's consider a concrete example, our [`DaysCompleted.js`](https://github.com/joeappleton18/web-dev-industry-practical/blob/master/src/Components/DaysCompleted.js). Below, I have refactored it using styles-components. 

```js
...
import styled from "styled-components";



function DaysCompleted(props) {
  const { days, checkins } = props;

  const DaysCompleteHeading = styled.h2`
    text-align: center;
    color: #bc9cff;
  `;

  const RootDiv = styled.div`
    display: grid;
    grid-template-columns: 0.8fr;
    grid-template-rows: 55px 80px 20px auto;
    justify-content: center;
  `;

  const GoalHeading = styled.h4`
    color: #1f2041;
  `;
  
  return (
    <Tile>
      <RootDiv>
        <DaysCompleteHeading> {days} Days Completed! </DaysCompleteHeading>
        <Histogram barCount={7} bars={checkins.map(c => c.score * 5)} />
        <ProgressBar />
        <GoalHeading>
          <strong>50%</strong> TO GOAL!
        </GoalHeading>
      </RootDiv>
    </Tile>
  );
}
...
```

This is very odd, what is happening above? 

We've componentized our styles. styled-components creates a new component for us that includes styling information.  Consider, in the above example, our `DaysCompleteHeading` :

```js
  const DaysCompleteHeading = styled.h2`
    text-align: center;
    color: #bc9cff;
  `;
```

Let's try and break this down. `styled.h2` is in-fact a function provided by `styled-components`.  There is a different function for every type of html element. We are then utilising tagged template literals (a recent addition to JavaScript) to write actual CSS code to style the h2 tag. Notice how everything in the "``" is just normal css. Also, note how we are writing CSS, this is not a JavaScript object literal.

:::tip

## Task 3  -  Styling our application

Use styled-components to replace the inline styles in `<Histogram>` and `<ProgressBar>`. 

Next, consider `<Tile >`,  use the documentation to work out how we might export a styled component, replacing our existing wrapped component. [Hint, you will need to pass in a prop to the styled component](https://styled-components.com/docs/basics#passed-props). Further, in our inline styles, currently `box-shadow` is camel cased to `boxShadow` to allow it to be used as a JavaScript object key. Remember, styled components use standard css not the camel cased alternative. 

:::

<HiddenSection display-text="Display solution to the new tile component">

  ```js
  import React from "react";
import PropTypes from "prop-types";
import styled from "styled-components";


const Tile = styled.div`box-shadow: 40px 10px 20px rgba(31, 32, 32, ${props => props.elevation});padding: 3%;`;


Tile.propTypes = {
  elevation: PropTypes.string
};

Tile.defaultProps = {
  elevation: "0.05"
};

export default Tile;

  
  
  ```

</HiddenSection>

## Global Styles 

So far, I hope that I have demonstrated that styled-components offer a powerful component level styling solution. However, you may well be thinking where do my global styles go? `styled-components` provides us with a `createGlobalStyle` module to solve this problem.

Let's add some global styles to our application. 



::: tip

## Task 4 - Creating Global Styles

First, we need to consider where our global styles should live. There are no strict opinions on how we should structure our react applications. It is very much down to you as a developer. 

I like to create a `src/config` folder and place my global styles there. 

As such, create the file and folder `src/config/GlobalStyles.js`. We can now start creating some global styles:

```js

import { createGlobalStyle } from 'styled-components'

const GlobalStyles = createGlobalStyle`

body {
  font-family: Quicksand;
}

h1 {
  font-size: 42px; 
}

h2 {
  font-size: 32px;
}

h3 {
  font-size: 32px;
}

h4 {
  font-size: 19px;
}

`

export default GlobalStyles;

```
Notice how GlobalStyles contains the current styling rules found in, `src/App.css`. Now we need to use these new styles, replacing  `src/App.css`:

At the top of `src/App.js`, import your GlobalStyles:

```js
import GlobalStyles from "./config/GlobalStyles";
```
Next, include the  GlobalStyles component to the top of your App components return statement:

```js
function App() {
  return (
    <div>
        <GlobalStyles />
        <DaysCompleted days={15} checkins={checkins}>
          {" "}
        </DaysCompleted>
      </ThemeProvider>
    </div>
  );
}
```

You can now remove the App.css at the top of `src/App.js`, as we have replaced it with a styled component.
::: 

## Themeing 

Let's revisit our style guide from earlier:

![](./assets/style_guide.jpg)

>> [Figure 1, the style guide for our tracking application](https://www.figma.com/file/rTbqRpRWOw7UYg28SBcxQv/web-dev-pratical-task-made-using-toxin-ui?node-id=31275%3A269)


One of the key features that really sold me on `styled-components` is it allows us to represent our style guide as a theme. The theme can be constructed as a simple object. 

:::tip 

## Task 5 - Themes 


Create `src/config/theme.js` and add the following code:

```js
const theme = {
    colors: {
        purple: '#BC9CFF'
    },
    typography: {
        fontFamily: 'Quicksand',
        h1: {
                fontSize: '42px'
        }
    }
}
export default theme;
```

Above, notice how I have begun to describe our style guide, using a simple JavaScript object literal - a data structure you already know. The const `theme` is intentionally lower case as it is not a component, this is just a naming convention I try to follow.  

`styled-components` provides us with a theme provider that will the theme object accessible within the styling rules.

In `src\App.js` import the theme and the `styled-components` ThemeProvider.

```js
  import theme from "./config/theme.js";
  import { ThemeProvider } from "styled-components";
```

Next, we need to wrap our entire application in the `ThemeProvider` that will take our `theme` as a prop. 


```js
function App() {
  return (
    <div>
      <ThemeProvider theme={theme}>
        <GlobalStyles />
        <DaysCompleted days={15} checkins={checkins}>
          {" "}
        </DaysCompleted>
      </ThemeProvider>
    </div>
  );
}
```

That's it! The theme now globally accessible and will be passed in as a prop to all my styled components. Let's use it, in `src/DaysCompleted.js`, update  `DaysCompleteHeading` to:

```js
 const DaysCompleteHeading = styled.h2`
    text-align: center;
    color: ${ props => props.theme.colors.purple};
  `;

```

In `src/config/GlobalStyles` make the following updates:

```js
`
body {
  font-family:  ${props => props.theme.typography.fontFamily}
}

h1 {
  font-size:  ${props => props.theme.typography.h1.fontSize} 
}
`
```
:::

Above, you can see that our theme now gets injected into any function that we use to set a value of our css. We can then return the theme value, or, if we want return any value based on the theme.

## Home Study/Overflow Tasks

:::tip

## Task 6 - Expanding on our Theme

Think about how we might expand on our theme so it better represents the style guide above. You can get as in-depth as you like here. For inspiration, you can consider this, huge, [theme object found in the material-ui library](https://material-ui.com/customization/default-theme/). Your theme will be a fraction of this size. However, you should always use inspiration. 

:::

:::tip

# Task 7 - Completing the Dash

Complete the `main_dash_view`:

![](./assets/main-dash.png)

:::















 
