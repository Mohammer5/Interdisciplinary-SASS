# Introduction

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
