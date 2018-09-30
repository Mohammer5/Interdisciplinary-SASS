# Interdisciplinary SASS

This is my book about how I write SASS code. I wrote it for people
who want to get more into writing large, scalable webapps or websites.

This book is available for free, neither do I not intend to make any profits with it
nor will I accept donations, it is truly and solely meant for the purpose of sharing my findings.

I won't and will never say that I've found a perfect solution. But at least I can share
what has worked best for me so far and hope that others, who struggle the same as I did,
might find some answers to questions in this book.

## Table of Contents

[* Introduction](Chapters/introduction.md)
[* Who this book is for](Chapters/TODO.md)
[* Prerequisites](Chapters/TODO.md)
[* CSS class naming convention](Chapters/TODO.md)
[* State, transformation, presentation](Chapters/State,\ Logic\ and\ Presentation.md)
[* Programming bottom up](Chapters/Programming\ bottom\ up.md)
[* Organizing a project](Chapters/TODO.md)
[* Testing the components](Chapters/TODO.md)
[* TDD in SASS](Chapters/TODO.md)
[* Example 1: Three-level navigation](Chapters/TODO.md)
[* Example 2: Shop product page sidebar](Chapters/TODO.md)
[* Final thoughts](Chapters/TODO.md)

## Introduction

As a software engineer, you always try to write perfect code. I think we all can admit that it
will never happen. Every developer knows the moment when they have to take a look at their
old code they were once very proud of and realise that they wish no one will ever see these
lines of code.

On my journey of being a software engineer, I’ve tried many different things in order to write
efficient, reusable, maintainable, reasonable and easy to read code. The range of what I
touched will not fit in a few lines here but to name a few - which probably the most of you
have tried as well - are different frameworks, design patterns like MVC, programming paradigms,
language supersets and many more.

Through the years of experimenting, crafting beautiful code that I later hated and trial and
error, I found a few concepts to be generally applicable to many different languages,
domains, even parts of my life outside of the programming world.

But for some reason, it took me quite some time to use these concepts when writing styles
for websites and web applications. All these years CSS was merely a tool I didn’t like to use
too much as it was very static and was more about memorizing rules rather than building
business logic. When SASS entered my world (when I speak of SASS, I always used SCSS,
which is a specific SASS syntax, but I’ll continue to speak of SASS as the principles remain
the same for the SASS language itself), I still didn’t try to look beyond the horizon.

It was actually the programming paradigm called functional programming or rather my
research to better understand this topic, that made me think about CSS a lot more. To me it
eliminates side effects as much as possible, introduces a greater level of abstraction but yet
made everything much simpler than I was used to when programming OOP software.

The technology I have used the most to handle the state of my applications is redux (using
both JavaScript and Typescript), both server and client side. In case of a client side
application, applications work in the following, very simplified way:

State is stored in one place, functions works with (not on!) the state in a pure, side-effect-free
way to transform the state to data which will be used to render HTML. So there are three
main parts: State Handling -> State Transformation -> Presentation

Of course this doesn’t apply to CSS the way it works in JavaScript, but I found a way how I
apply this mindset to CSS and it has worked great for me. Besides that, the other crucial
concept I apply everytime writing code is called: “Programming bottom up”.

## Who this book is for
This book is meant for professional front end engineers who work with CSS on a daily basis.
I will list some topics (including links if you haven’t heard of or studied these topics yet)
which will be treated as if they are known the to reader of this book already.

If you are interested in improving the scalability and maintainability of the
styling of your application, and have worked on big projects already,
then this book is for you.

### Who this book is not for
If you just started learning learning CSS or want to get an overview of what CSS is and
what it’s capable of, then this book is not for you

## Prerequisites
There are a few topic you should be familiar with before you start reading this book.

### Pure functions and side effects
This is a topic from outside the CSS world. You can find many blog articles
just about these two topics. I recommend using your search engine of choice and
read a few articles about them until you think you understand them,
could apply them in a project right away and see the benefits
(especially in regards to testability of your code).

### Programming bottom up
The article I recommend to read about this topic is written by Paul Graham.
You can find it on his website or by simply using a search engine and look for
“Paul Graham Programming bottom up”.

### BEM
BEM introduced a naming convention for css classes to improve CSS performance.
Although the syntax can look a bit ugly, it has worked great for me so far!
You can read more about this on BEM’s website.

### SMACSS
I like SMACSS mainly for its separation of CSS into problem domains.
The pages under the category “Core” are available on the website of its inventor.

CSS class naming convention
Before the times of HTML5, the <hr>-tag was used to insert a horizontal rule.
The reason to remove this functionality in HTML5 is that HTML should be about semantic,
not about styling. It’s best to express what the content is about,
not what it should look like as that’s the purpose of CSS (it’s harder to maintain as well,
but I suppose that you know the principle “Separation of concerns” already).

Because of this, I think that class names should not describe what the element it’s applied to
should look like but rather describe the contents. You might think that this eliminated reusability
s you can’t reuse class names for different contents, but we’ll talk more about that later.

Let’s have a look at a few examples

```HTML
<div class="secondary-information">
  <h3 class="secondary-information__headline">
    More information about this topic
  </h3>

  <ul class="secondary-information__headline link-list">
    <li class="link-list__item">
      <a class="link-list__link" href="https://domain1.tld">
        More info 1
      </a>
    </li>
    <li class="link-list__item">
      <a class="link-list__link" href="https://domain2.tld">
        More info 2
      </a>
    </li>
  </ul>
</div>```

As you can see I used classes to describe the contents. This could be a box in a sidebar
to show more information to the topic, but I didn’t gave it the name “box”
as that wouldn’t say anything about the box. I didn’t choose a name though
that describes the exact contents of this box. If this box needs specific
styles because of its contents, then this should be done with a modifier.
I. e. you can have a box with secondary information about browser quirks
which you want to highlight explicitly, then you can add the class name
“secondary-information--browser-quirks” to the main block (BEM) html element.

There can be different boxes for the sidebar with different purposes and
therefore different class names and you might think that in that case we
have to apply the same styles to different class selector and you’re right.
But that doesn’t mean that we’ll have duplicate css rules. I’ll talk more about
this topic in the chapters “Programming bottom up” and “Organizing a project”.

By working this way it is significantly easier to implement small differing
details between boxes that look more or less the same without having to come up
with complicated, abstract names for the class names. The reader of your code
(which could be future-you) will know what the element and its styling is about
and - if you organized your project in a good way (more about this in the
chapter “Organizing a project”) - where to find the style rules and adjust them
without any hassle.

```HTML
<div class="video-actions">
  <span class="video-actions__video-actions video-actions--toggle-play">
    Toggle play
  </span>
  <span class="video-actions__video-actions video-action--stop">
    Stop
  </span>
  <span class="video-actions__video-actions video-action--volume-switch">
    Toggle mute
  </span>
  <span class="video-actions__video-actions video-action--toggle-fullscreen">
    Toggle fullscreen
  </span>
</div>

This is a simplified action bar of a video player. It contains a few standard
actions the viewer can perform while watching a video. The class names for
buttons themselves are not called “action-button” as it’s describing what it’s
going to look like (or in this case, how it’s going to be working -> A button
means clickable).

If I named the buttons “video-action-button” and I got the requirement from the
UX department to change the toggle mute button into a mix of a button and a bar
so that the viewer can change the volume as well, the name “button” wouldn’t be
appropriate anymore.

By avoiding describing the style or intended functionality, whenever the
styling or functionality changes, there’s no need to adjust the css. 

## State, Logic and Presentation

After having worked with Redux and React in the JavaScript world
for some time, I really like the concept of storing all the data
in one place combined with a unidirectional dataflow. I never had
more control over my applications and keeping the structure simple
and reasonable kinf od just happened.

I have copied this way of thinking when styling webapps.
And although it's not the same and more an abstraction layer in
my head, it has worked great for me.

### State

In normal application that work the way I've desribed above,
the state contains data which can change over time, i. e. 
when the user enters a name in a form field. In CSS this 
will probably never happen (who knows what the future and
the mind of a genius might bringt).

In CSS, the Design and Styleguide of the Design/UX department
are my state. They define what the webapp should look like.

It can and very likely will change over time, it will be extended,
and - when working for an agency - many Design patterns (in terms of the Design)
of an older project will be copied to new projects while adjusting
mostly colors and images only.

In JavaScript (or other languages), the application state will determine
what the HTML will look like. When working with CSS,
the design will determine what the CSS will look like.
The developer has to translate into styleguide css code.

The styleguide css code can be used to create a css- and html-file. The rendered output
can be tested with cypress, cucumberjs (my favourite as the Design/UX department can write the specs),
or any other tool of your choice.

*The styleguide css code doesn't contain any actual css rules,
it should contain variables, mixins and functions only!
In order to test the styleguide with tests, a seperate index file will be required*
(I'll talk about seperate index files in [a later chapter](@TODO))

## Programming bottom up

Programming bottom up is the core skill for good abstraction layers.
And as it's useful when writing code for software applications, it's also
very useful when it comes to writing CSS code. So let's have a look at
what methods of abstraction in CSS we have

### Variables

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

### Mixins

Mixins allow us to separate "presentation" css code (see the Chapter "State, logic, presentation")
into reusable containers. Like functions they can expect parameters
