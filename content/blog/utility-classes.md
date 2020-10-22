---
title: Utility Classes
date: "2020-10-22T15:32:44.604Z"
description: "A reflection on why utility casses maybe aren't so great"
---

Lately there appears to be a profound interest in CSS utility classes. Maybe this is just due to the [Baader-Meinhof Phenomenon](https://www.healthline.com/health/baader-meinhof-phenomenon)
but a popular library -- [tailwindcss](https://tailwindcss.com/) -- is seemingly everwhere! tailwindcss argues that CSS frameworks do
too much and since they offer predesigned components, building is initially fast but comes with a higher price later on. So tailwindcss's solution is
a bunch of low-level CSS classes that "let you build completely custom designs without ever leaving your HTML." They aren't the only
library to offer this though. Bootstrap -- arguably the framework tailwindcss is accusing of "doing too much" -- has these same
set of utility classes.

This all sounds great... but I don't get it.

I would argue that this method of applying a bunch of classes to an element will eventually lead to a higher cost than designing a robost component.
Even using a component library and overriding the styles would be cheaper down the road.

Requirement: "Make a universal button. This button will be used as the primary action thoughout the application"


```html
<button class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">
  Button
</button>
```

This can be wrapped in reusable component in your framework of choice. I'm going to use React here because of its popularity.

```jsx
// React
<PrimaryButton>Add New Customer</PrimaryButton>
```

But what benefit do utility classes have over classes or even inline styles?

```jsx

const PrimaryButton = ({ children }) => {
  return (
    <button styles={{ backgroundColor: 'blue', color: 'white', fontWeight: 'bold', paddingTop: '2rem', paddingBottom: '4rem' }}>{children}</button>
  );
}
```

Conveniently left out of the inline-style example is the `:hover`. This isn't really feasible with inline-styles and would require using
plain old CSS, CSS imports, a css-in-js solution, etc. So that could be considered a positive for the utility classes. Another argument in
favor of the utility classes is that they are easier to read and definitley more succint (`py-2` vs `paddingTop: '2rem'`).

So now onto some of the pitfalls.

What happens when a designer wants to use a color that isn't predefined? You can't just use `class="bg-#390970`. One solution is to configure
the utility class library. Another is to one-off the style or another custom class

```jsx
<button style="{ background-color: #390970 }" class="text-white font-bold py-2 px-4 rounded">
  Button
</button>

/**
*
*  index.css
*
*  .primary-button {
*    background-color: #390970;
*  }
*
*/

<button class="primary-button text-white font-bold py-2 px-4 rounded">
  Button
</button>
```

While changing the configuration of the library will do the trick it may be unfamiliar territory for designers or at the very least
would limit the options available because there are only so many configuration options avialble.

As for inling the styles why can't all the styles referenced by the class be inlined as well at that point? Same thing goes for the class.
Just define all the styles required for the button in `primary-button`.

I will say that tailwindcss provides a bunch of configuration options and it's written in JavaScript so you can extend pretty much anything.

What does this have over a designer writing an application specific CSS stylesheet?

If the components are not resused (say in a plain HTML page) there is a problem when the designer asks to update the style of a button: you have
to change it everywhere. While this probably isn't such a big deal since most large applications now have the concept to create
reusable components it is still something to consider. For instance, angular does not make creating new components very easy. It's common
to see developers simply copy portions of HTML templates. When this happens updating a style across the board is not straightforward.
If the developer used a more global style like `.primary-button` updating it in 1 stylesheet is much more efficient and painfree than
grepping for `btn` and deciding if it is indeed a primary button or not and making the necessary adjustments.