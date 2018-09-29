# Programming bottom up

Programming bottom up is the core skill for good abstraction layers.
And as it's useful when writing code for software applications, it's also
very useful when it comes to writing CSS code. So let's have a look at
what methods of abstraction in CSS we have

## Variables

This is a fairly obvious one. With variables we can store values and reuse them.
But we can bring this to the next level!

Most CSS styles of websites and webapps follow a style guide.
If the style you're working in doesn't, you should watch the presetation
[NAME](@TODO) by [NAME](@TODO) on Youtube[FOOTNOTE](@TODO).

All stylesguides make use of different colors for different intentions.
Let's assume your styleguide uses the following colors as the colorpalette:

Blue, red, yellow, green and orange.
White, alto and grey, black.

All these colors will be used to express different intentions,
i. e. the color yellow will be used as primary actions,
grey for all other actions and alto for disabled actions.

When you start working on the project, these colors are not used for anything else.
So you could store them in variables descibing their use cases, like:

```CSS
$action-primary: #00ff00;
$action-default: #808080;
$action-disabled: #ddd;
```

This would give you the flexibility of reusing the colors whenever a certain 
action color type is required and you can easily change the color in one file
in order to change the color everywhere.

But one the one hand the next developer looking at that code doesn't know which colors were used
(unless the IDE or Editor shows the color ) and on the other hand that way solves only one problem domain.
Assigning these colors to specific action colors.
If the design system will be extended in the future and these colors
are going to be used for something entirely different, in most cases,
developers do one the following two options:

First, just use the action color variable to assign the color to something else,
e. g. the footer background.

Second, create a new variable with the same color value to not reuse a variable which might
change for a reason which has nothing to do with the new use case. Which introduces the problem again,
that the next developer looking at this code doesn't know which color is actually used until he looks
either at the webapp itself or looks up the color code.

That approach is called Programming top down. You create a solution for a specific problem domain.
But as you can see this has several disadvantages, here's another solution for this:

```CSS
// First we define the colors themself
$blue: #0000ff;
$red: #ff0000;
$yellow: #00ff00;
$green: #00ffff;
$orange: #ffff00;
$white: #fff;
$alto: #ddd;
$grey: #808080;
$black: #000;

// Then we define the action colors
$action-primary: $yellow;
$action-default: $grey;
$action-disabled: $alto;

// If we need the colors somewhere else again, we can reuse the names colors
$footer-bg: $grey;
```

This way the cognitive load of the developers reading the code is reduced quite a bit,
as they see which color is used right away.
Also the chance that a developer reuses a variable in a bad way is reduced.

Obviously I just gave the color codes a name so I can reuse the name instead of the code directly.
So what does this have to do with Programming bottom up.

I've split the actual problem domain into several problem domains and composed those
lower level problem domains to solve the actual problem domain.
This was a very simple problem, we'll get to more complex problems with mixins and functions
Instead of asking myself which colors the actions need, I first had to solve the Problem
of where I get the colors from.

## Mixins

Mixins allow us to separate "presentation" css code (see the Chapter "State, logic, presentation")
into reusable containers. Like functions they can expect parameters
