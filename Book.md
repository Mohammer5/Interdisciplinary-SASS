# Interdisciplinary SASS

This is my book about how I write SASS code. I wrote it for people
who want to get more into writing large, scalable webapps or websites.

Copyright © 2010 by Jan-Gerke Salomon

You may clone the git repository and make any changes to the content
in order to submit a pull request.

## Table of Contents

* [Introduction](#chapter-intro)
* [Who this book is for](#chapter-who-is-for)
* [Foreknowledge](#chapter-foreknowledge)
* [CSS class naming convention](#chapter-naming-convention)
* [State, transformation, presentation](#chapter-stp)
* [Programming bottom up](#chapter-pbu)
* [Organizing a project](#chapter-organize)
* [Testing the components](#chapter-testing)
* [TDD in SASS](#chapter-tdd)
* [Example 1: Three-level navigation](#chapter-example-1)
* [Example 2: Shop product page sidebar](#chapter-example-2)
* [Final thoughts](#chapter-final-thoughts)

<a name="chapter-intro"></a>
## Introduction

As a software engineer, you always try to write perfect code. I think we all
can admit that it will never happen. Every developer knows the moment when
they have to take a look at their old code they were once very proud of and
realise that they wish no one will ever see these lines of code.

On my journey of being a software engineer, I’ve tried many different things in
order to write efficient, reusable, maintainable, reasonable and easy to read
code. The range of what I touched will not fit in a few lines here but to name
a few - which probably the most of you have tried as well - are different
frameworks, design patterns like MVC, programming paradigms, language supersets
and many more.

Through the years of experimenting, crafting beautiful code that I later hated
and trial and error, I found a few concepts to be generally applicable to many
different languages, domains, even parts of my life outside of the programming
world.

But for some reason, it took me quite some time to use these concepts when
writing styles for websites and web applications. All these years CSS was
merely a tool I didn’t like to use too much as it was very static and was
more about memorizing rules rather than building business logic. When SASS
entered my world (when I speak of SASS, I always used SCSS, which is a specific
SASS syntax, but I’ll continue to speak of SASS as the principles remain the
same for the SASS language itself), I still didn’t try to look beyond the
horizon.

It was actually the programming paradigm called functional programming or
rather my research to better understand this topic, that made me think about
CSS a lot more. To me it eliminates side effects as much as possible,
introduces a greater level of abstraction but yet made everything much simpler
than I was used to when programming OOP software.

The technology I have used the most to handle the state of my applications is
redux (using both JavaScript and Typescript), both server and client side. In
case of a client side application, applications work in the following, very
simplified way:

State is stored in one place, functions works with (not on!) the state in a
pure, side-effect-free way to transform the state to data which will be used to
render HTML. So there are three main parts: State Handling -> State
Transformation -> Presentation

Of course this doesn’t apply to CSS the way it works in JavaScript, but I found
a way how I apply this mindset to CSS and it has worked great for me. Besides
that, the other crucial concept I apply everytime writing code is called:
“Programming bottom up”.

<a name="chapter-who-is-for"></a>
## Who this book is for
This book is meant for professional front end engineers who work with CSS on a
daily basis. I will list some topics (including links if you haven’t heard of
or studied these topics yet) which will be treated as if they are known the to
reader of this book already.

If you are interested in improving the scalability and maintainability of the
styling of your application, and have worked on big projects already,
then this book is for you.

### Who this book is not for
If you just started learning learning CSS or want to get an overview of what
CSS is and what it’s capable of, then this book is not for you

<a name="chapter-foreknowledge"></a>
## Foreknowledge
There are a few topic you should be familiar with before you start reading this
book.

I'll have a few examples with typescript and/or react/jsx. Don't worry if
you're not familiar with those languages/libraries, it should be fairly
simple to understand the meaning without having to learn the lang/lib.

### Pure functions and side effects
This is a topic from outside the CSS world. You can find many blog articles
just about these two topics. I recommend using your search engine of choice and
read a few articles about them until you think you understand them,
could apply them in a project right away and see the benefits
(especially in regards to testability of your code).

Tl;dr... It basically means that you write your code in a mathematical way.
In school we learned about functions in math. They always produced the same
result for the same given input. They never influenced anything outside of
the function nor did they change the input parameters.
It's the same concept when you write pure functions that are free of side
effects, there are a few rules to this:

1. You never change the input parameters, treat them as immutable
2. You never access anything from outside of the function's scope, except
for other pure functions
3. You never change anything outside of the function's scope, treat it as
inaccessible
4. You always return the same output for a given input

Let's have a look at a few impure functions

Example 1
```typescript
interface User {
	name: string;
	online: boolean;
}

type GetOnlineUsers = (prefix?: string): User[];
const getOnlineUsers: GetOnlineUsers =
	(prefix = '') =>
		fetch(`${prefix}/users/?filter=online`);
```

As you can see here, we do an ajax request which returns a list of online
users. As the result varies, depending on who's online, this functions is
clearly impure.

Example 2
```typescript
interface User {
	name: string;
	online: boolean;
}

type AppendUser = (users: User[], user: User) => User[];
const appendUser: AppendUser =
	(users, user) => {
		users.push(user);
		return users;
	};
```

In this example, I'm modifying the input parameter, so this function can't be
pure as this is not allowed.

Example 3
```typescript
const createPage = (path, globals = {}) => {
	if (path.match(/^\/user/)) {
		window.page = new UserPage(globals);
	} else {
		window.page = new Page(globals);
	}
}
```

Modifying anything outside of the function's scope is impure, so this function
doesn't qualify for pure functions.

### Programming bottom up
The article I recommend to read about this topic is written by Paul Graham.
You can find it on his website or by simply using a search engine and look for
“Paul Graham Programming bottom up”.

To summarize this shortly: The most common approach to problems is "Programming
top down". It describes solving a problem by implementing what is needed in
order to solve the problem and nothing else. Programming bottom up is the 
opposite of that. Instead of solving the problem, the developer has to split
the problem into sub-problems, which he has to split into more sub-problems
until he found all the root problems which need to be solved in order to solve
the actual problem.

Here's a short (stupid) example: Your webapp displays a list of books but lacks
an option to exclude genres. The top down solution could look like this:
(I use typescript here so that the input types are clear)

```typescript
type Genre = string;
interface Book {
	genres: Genre[];
	name: string;
	// more..
}

const excludeGenresFromBookList =
	(books: Book[], genres: Genre[]) => books.filter(
		book => genres.filter(
			genre => book.genres.contains(genre),
		),
	);
``` 

This would probably work but none of this is reusable in any way.
The bottom up approach could look like this:

```typescript
import {
	any,
	contains,
	equals,
	filter,
	partial,
	partialRight,
} from 'ramda';

type IsTrue = (val: boolean): boolean;
const isTrue: IsTrue =
	equals(true);

type ExtractGenresFrom = (obj: { genres: Genre[] }) => Genre[];
const extractGenresFrom: ExtractGenresFrom =
	prop('genres');

type HasGenre = (book: Book, genre: Genre) => boolean;
const hasGenre: HasGenre =
	(book, genre) => contains(
		genre,
		extractGenresFrom(book),
	);

type MapBookHasGenres = (book: Book, genres: Genre[]) => boolean[];
const mapBookHasGenres: MapBookHasGenres =
	(book, genres) => map(
		partial(hasGenre, [ book ]),
		genres,
	);

type HasOneOfGenres = (book: Book, genres: Genre[]) => boolean;
const hasOneOfGenres: HasOneOfGenres =
	(book, genres) => any(
		isTrue,
		mapBookHasGenres(book, genres),
	);

type HasNoneOfGenres = (book: Book, genres: Genre[]) => boolean;
const hasNoneOfGenres: HasNoneOfGenres =
	(book, genres) => pipe(
		partialRight(doesBookHaveOneOfGenres, [ genres ]),
		not,
	);

type ExtractBooksWithoutGenres = (books: Book[], genres: Genre[]) => Book[];
const extractBooksWithoutGenres =
	(books, genres) => filter(
		hasNoneOfGenres,
		books,
	);
```

To be fair, this is a lot more code and it takes a lot more time to write that
amount of code. You'll have to come up with names, split problems into other
problems, write more tests.
But, the next time you'll have to check for truthyness, get genres from a book,
refactor the way genres are stored in a book, check if one book matches one or
more genres, you won't have to implement that again. It's already there. It
enables and/or simplifies function composition, reduces cognitive load when
reading code and improves maintainability.

It's the same philosophy as with TDD. It takes more time initially, but when
either have to work with the code again or have to refactor some code,
you'll be thankful that everything is already in place.

### BEM
BEM introduced a naming convention for css classes to improve CSS performance.
Although the syntax can look a bit ugly, it has worked great for me so far!
You can read more about this on BEM’s website.

It's basically like this: You compose your HTML with modules. The root
container of that module is a block (that's the B in BEM). Your module probably
consists of multiple parts, e. g. a headline, content and/or footer. These are
elements (the E in BEM). Some modules can have variations, which will be done
with modifiers (the M in BEM).

The class name of elements is the block name, followed by to underscores and
the element name. Modifiers are either the block or element class name folowwed
by two dashes and the name of the modifier

Example 1

```html
<div class="contact-list">
	<h3 class="contact-list__headline">
		Contacts
	</h3>

	<ul class="contact-list__contacts">
		<li class="contact-list__contact">
			Firstname1 Lastname1,<br>
			<a href="tel:07891234560">0789 123456-0</a>
		</li>
		<li class="contact-list__contact">
			Firstname2 Lastname2,<br>
			<a href="tel:07891234561">0789 123456-1</a>
		</li>
	</ul>
</div>

<div class="contact-list contact-list--international">
	<h3 class="contact-list__headline">
		International Contacts
	</h3>

	<ul class="contact-list__contacts">
		<li class="contact-list__contact">
			Firstname3 Lastname3,<br>
			<a href="tel:+61987123456">+61 987 123456</a>
		</li>
		<li class="contact-list__contact contact-list__contact--no-english">
			Firstname4 Lastname4 (local language only),<br>
			<a href="tel:+636341234560">+63 634 123456-0</a>
		</li>
		<li class="contact-list__contact">
			Firstname5 Lastname5,<br>
			<a href="tel:+636341234561">+63 634 123456-1</a>
		</li>
	</ul>
</div>
```

Here the module for displaying contacts has a modifier for international
contacts. It could have a slightly different background color.
There's also one international contact who can't speak english, which
might be highlighted in some way.

### SMACSS
I like SMACSS mainly for its separation of CSS into problem domains.
The pages under the category “Core” are available on the website of its inventor.

<a name="chapter-naming-convention"></a>
## CSS class naming convention
Before the times of HTML5, the `<hr>`-tag was used to insert a horizontal rule.
The reason to remove this functionality in HTML5 is that HTML should be about
semantics, not about styling. It’s best to express what the content is about,
not what it should look like as that’s the purpose of CSS (it’s harder to
maintain as well, but I suppose that you know the principle “Separation of
concerns” already).

Because of this, I think that class names should not describe what the element
it’s applied to should look like but rather describe the contents. You might
think that this eliminated reusability as you can’t reuse class names for
different contents, but we’ll talk more about that later.

Let’s have a look at a few examples

```html
<div class="secondary-information">
  <h3 class="secondary-information__headline">
    More information about this topic
  </h3>

  <div class="secondary-information__content">
    <ul class="link-list">
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
  </div>
</div>
```

As you can see I used classes to describe the contents. This could be a box in
a sidebar to show more information to the topic, but I didn’t gave it the name
“box” as that wouldn’t say anything about the box. I didn’t choose a name
though that describes the exact contents of this box. If this box needs
specific styles because of its contents, then this should be done with a
modifier. I. e. you can have a box with secondary information about browser
quirks which you want to highlight explicitly, then you can add the class name
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

```html
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
```

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

<a name="chapter-stp"></a>
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
and - when working for an agency - many Design patterns (in terms of the
Design) of an older project will be copied to new projects while adjusting
mostly colors and images only.

In JavaScript (or other languages), the application state will determine
what the HTML will look like. When working with CSS,
the design will determine what the CSS will look like.
The developer has to translate into styleguide css code.

The styleguide css code can be used to create a css- and html-file. The
rendered output can be tested with cypress, cucumberjs (my favourite as the
Design/UX department can write the specs), or any other tool of your choice.

*The styleguide css code doesn't contain any actual css rules,
it should contain variables, mixins and functions only!
In order to test the styleguide with tests, a seperate index file will be
required* (I'll talk about seperate index files in [a later chapter](@TODO))

<a name="chapter-pbu"></a>
## Programming bottom up

Programming bottom up is the core skill for good abstraction layers.
And as it's useful when writing code for software applications, it's also
very useful when it comes to writing CSS code. So let's have a look at
what methods of abstraction in CSS we have

### Variables

This is a fairly obvious one. With variables we can store values and reuse
them. But we can bring this to the next level!

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

When you start working on the project, these colors are not used for anything
else. So you could store them in variables descibing their use cases, like:

```scss
$action-primary: #00ff00;
$action-default: #808080;
$action-disabled: #ddd;
```

This would give you the flexibility of reusing the colors whenever a certain 
action color type is required and you can easily change the color in one file
in order to change the color everywhere.

But one the one hand the next developer looking at that code doesn't know which
colors were used (unless the IDE or Editor shows the color ) and on the other
hand that way solves only one problem domain. Assigning these colors to
specific action colors. If the design system will be extended in the future and
these colors are going to be used for something entirely different, in most
cases, developers do one the following two options:

First, just use the action color variable to assign the color to something
else, e. g. the footer background.

Second, create a new variable with the same color value to not reuse a variable
which might change for a reason which has nothing to do with the new use case.
Which introduces the problem again, that the next developer looking at this
code doesn't know which color is actually used until he looks either at the
webapp itself or looks up the color code.

That approach is called Programming top down. You create a solution for a
specific problem domain. But as you can see this has several disadvantages,
here's another solution for this:

```scss
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

This way the cognitive load of the developers reading the code is reduced quite
a bit, as they see which color is used right away. Also the chance that a
developer reuses a variable in a bad way is reduced.

Obviously I just gave the color codes a name so I can reuse the name instead of
the code directly. So what does this have to do with Programming bottom up.

I've split the actual problem domain into several problem domains and composed
those lower level problem domains to solve the actual problem domain. This was
a very simple problem, we'll get to more complex problems with mixins and
functions Instead of asking myself which colors the actions need, I first had
to solve the Problem of where I get the colors from.

### Mixins

Mixins allow us to separate "presentation" css code (see the Chapter "State,
logic, presentation") into reusable containers. Like functions they can expect
parameters.
