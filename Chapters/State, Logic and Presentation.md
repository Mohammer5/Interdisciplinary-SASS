# State, Logic and Presentation

After having worked with Redux and React in the JavaScript world
for some time, I really like the concept of storing all the data
in one place combined with a unidirectional dataflow. I never had
more control over my applications and keeping the structure simple
and reasonable kinf od just happened.

I have copied this way of thinking when styling webapps.
And although it's not the same and more an abstraction layer in
my head, it has worked great for me.

## State

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
