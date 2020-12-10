# SASS Fundamentals

SASS is a css extension

There are 2 syntaxs supported by SASS

1. SCSS Syntax (.scss) (Sassy CSS - Super Set of CSS (all valid css is valid scss)) the most popular syntax
2. Indented Syntax (.sass) uses indentations instead of curly braces

`&` = parent

```scss
.main {
  width: 80%;
  margin: 0 auto;

  #{&}__paragraph {
    font-weight: map-get($font-weights, bold);

    &:hover {
      color: pink;
    }
  }
}
```

`&` = parent (in the above example &=main)
So the html class would be `main__paragraph`

Why did we have to use `#{&}`

If we did:
```scss
.main {
  width: 80%;
  margin: 0 auto;

  &__paragraph {
    font-weight: map-get($font-weights, bold);

    &:hover {
      color: pink;
    }
  }
}
```

The css would be:
```css
main__paragraph {
  font-weight: 700;
}* {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: #272727;
  color: #fff;
}

.main {
  width: 80%;
  margin: 0 auto;
}

.main .main__paragraph {
  font-weight: 700;
}

.main .main__paragraph:hover {
  color: red;
}
/*# sourceMappingURL=main.css.map */
```
And thats its own style **not within main (parent)**

So to fix it we use `#{&}` - this is called interpolation

Interpolation means -> we dont just want `.main__paragraph` we want everything before it

So with interpolation the css now looks like this:

```scss
.main .main__paragraph {
  font-weight: 700;
}
```

With Sass we can create partial sass files that contain little snippets of CSS that you could include in other Sass files

So if we're working on a large project it's a great way to modularize your css and make things easier to maintain.

A partial file is simply a sass file named with a leading underscore. So the underscore lets sass know that the file is only a partial and should not generate a css file so the compiler is going to ignore those  files that begin with an underscore

So to use partial files, so go to the selected scss file we want to use the partial with and `import` it:

```scss
@import './resets';

$primary-color: #272727;
$accent-color: #ff652f;
$text-color: #fff;
$font-weights: (
  "regular": 400,
  "medium": 500,
  "bold": 700,
);

body {
  background: $primary-color;
  color: $text-color;
}

.main {
  width: 80%;
  margin: 0 auto;

  #{&}__paragraph {
    font-weight: map-get($font-weights, bold);

    &:hover {
      color: red;
    }
  }
}
```

and then our css will look like this:
```css
* {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: #272727;
  color: #fff;
}

.main {
  width: 80%;
  margin: 0 auto;
}

.main .main__paragraph {
  font-weight: 700;
}

.main .main__paragraph:hover {
  color: red;
}
/*# sourceMappingURL=main.css.map */
```

Functions

`@function functionName`

Before:
```scss
$font-weights: (
  "regular": 400,
  "medium": 500,
  "bold": 700,
);

  #{&}__paragraph {
      font-weight: map-get($font-weights, bold);
```

After:
```scss
@function weight($weight-name) {
  @return map-get($font-weights, $weight-name);
}

  #{&}__paragraph {
    font-weight: weight(bold);
```

`@mixin`

Mixins allow you to reduce redundancy in your styling...

For example:

```scss
.main {
  display: flex;
  justify-content: center;
  align-items: center;

  width: 80%;
  margin: 0 auto;

  #{&}__paragraph {
    font-weight: weight(bold);

    &:hover {
      color: pink;
    }
  }
}
```

We don't want to write those 3 lines everytime we need a flex box, so we create a mixin

```scss
@mixin flexCenter($direction) {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: $direction;
}
```

It accepts arguments as well

then we add our mixin to whatever we're styling

```scss
.main {
  @include flexCenter(row);

  width: 80%;
  margin: 0 auto;

  #{&}__paragraph {
    font-weight: weight(bold);

    &:hover {
      color: pink;
    }
  }
}
```

`mixins` and `functions` are very similar but

functions should be used to compute and return values, mixins should define styles

A perfect example for a mixin is choosing between a light and dark theme

We use them by first creating the mixin **then** we do `@include mixinName`

```scss
@mixin theme($light-theme: true) {
  @if $light-theme {
    background: lighten($primary-color, 100%);
    color: darken($text-color, 100%);
  }
}

.light {
  @include theme($light-theme: false);
}
```

And for demonstration purposes add the `light` class to the body tags and you'll see!

Then you can toggle true and false to see the difference!

```scss
@mixin mobile {
  @media (max-width: $mobile) {
    @content;
  }
}
```

`@content` will be called when the mixin mobile is called


Parent Selectors

```html
<html>
  <head>
    <link rel="stylesheet" href="app.css" />
  </head>
  <body>
    <h1>Beautiful Buttons</h1>
    <!-- BEGIN EXERCISE -->
    <p>
      <button class='btn btn-primary'>Click Me!</button>
      <label>Primary</label>
    </p>

    <p>
      <button class='btn btn-secondary'>Click Me!</button>
      <label>Secondary</label>
    </p>

    <p>
      <button class='btn btn-primary' disabled>Click Me!</button>
      <label>Primary Disabled</label>
    </p>

    <p>
      <button class='btn btn-secondary' disabled>Click Me!</button>
      <label>Secondary Disabled</label>
    </p>
<!--
    <h2>Inputs</h2>
    <p>
      <input type="text" value='Hello Text'>
      <label>Text</label>
    </p>
    <p>
      <input type="password" value='Hello Password'>
      <label>Password</label>
    </p>
    <p>
      <input type="range" value='Hello Range'>
      <label>Range</label>
    </p>-->
    <!-- END EXERCISE -->
    <script src="js/tester.js"></script>
    <script src="js/utils.js"></script>
    <script src="tests.js"></script>

  </body>
</html>
```

Notice that we have classNames that are `btn btn-secondary`

this allows us to write our `scss` as:

```scss
.btn {
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 2px;
  padding-bottom: 2px;
  border: 1px solid #c46;
  border-radius: 2px;

  &-primary {
    background-color: #c46;
    color: #fff;
  }

  &-secondary {
    background-color: #edbcc8;
    color: #000;
  }
}
```

or 

```scss
body {
    h1 {
    color: #c46;
  }
}

.btn {
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 2px;
  padding-bottom: 2px;
  border: 1px solid #c46;
  border-radius: 2px;

  &.btn-primary {
    background-color: #c46;
    color: #fff;
  }

  &.btn-secondary {
    background-color: #edbcc8;
    color: #000;
  }

  &:disabled { 
    opacity: 0.5;
  }

}
```

However if we write as `&-primary` instead of `.btn-primary`  -- `btn btn-primary` and `btn btn-secondary` are no longer scoped to the `btn` class anymore in our output css

@import

partials are designed to be imported into other scss files and they will not become their own css files
Syntax: _filename.scss = the underscore lets css compiler know to not make it into a css file

Variables

Global Variable Declaration: `$color: #fff` this variable is now available anywhere it was imported or anywhere in the file it was declared

Comments
```scss
/**
 * 
 */
```

Comment blocks are preserved and will show up in the css file
But inline comments will not


