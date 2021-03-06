dust-js-browser
===============

A sample of using [DustJS](http://akdubya.github.io/dustjs/) in the browser.  (non-NodeJS example)

View the example at http://ericktai.github.io/dust-js-browser/

I created this repo as a reference point on using the DustJS templating library without NodeJS.  The Dust and LinkedIn tutorials didn't really quite describe how to get things running in a regular HTML/JS/Browser app, so this was an exercise in doing so.

## Getting Started

The pre-compiled JS is ready to go - just open `index.html` in the browser and find the template at `src/dusts/todos.dust` to see how DustJS works.

Making changes?  Run `node duster.js` in your root folder on the command line first.  It watches and compiles your templates to JS as needed.  You'll need `duster.js`. See <a href="https://github.com/dmix/dusterjs#install" target="_blank">duster.js</a> for more info.

## How It Works

DustJS templates should be saved to `src/dusts` as `*.dust` files.  DustJS templates are compiled into `*.js` files - use [duster.js](https://github.com/dmix/dusterjs) to auto compile your templates to `templates/compiled/*.js`.  I've configured the source and compiled paths via my [duster.json](https://github.com/ericktai/dust-js-browser/blob/master/duster.json) file.

You can use your template in `index.html`.  In this example, my template is `todos.dust`, and it was compiled into `todos.js`.  Let's include the template in the page.

```js
<script type="text/javascript" src="http://codeorigin.jquery.com/jquery-1.10.2.min.js"></script>
<script type="text/javascript" src="dust-core-2.0.3.js"></script>
<script type="text/javascript" src="templates/compiled/todos.js"></script>
```

As my example dust template is `todos.js`, dust will give it a name of `todos`.  The following binds a given JSON (details not shown) to the `todos` template.

```js
dust.render("todos", {
  //This 2nd parameter is the JSON data you want to bind with the template
}, function(err, out) {
  //when done binding the template, the HTML will be represented by the "out" variable
});
```

The template runs against JSON that you pass in.  Here's my JSON data with two `todo` objects:

```js
{
  listname: 'My Todos',
  todos: [
    {
      action: 'learn some dust!',
      duedate: 'today'
    },
    {
      action: 'visit the south bay',
      duedate: 'tomorrow'
    }
  ]
}
```

Combined:

```js
dust.render("todos", {
	listname: 'My Todos',
	todos: [
		{
			action: 'learn some dust!',
			duedate: 'today'
		},
		{
			action: 'visit the south bay',
			duedate: 'tomorrow'
		}
	]
}, function(err, out) {
  //when done binding the template, the HTML will be represented by the "out" variable
});
```

Finally, the output is handled by the function in the 3rd parameter: `dust.render(..., ..., function(err, out) {})`.  `out` represents the processed HTML result.  To set that to a `div`, simply use something like jQuery to do so:

```js
dust.render("todos", {
	//JSON data...
}, function(err, out) {
  $('#content').html(out);
});
```

Here's the `*.dust` template so you can see how the JSON binds to it:

```
<h1>{listname}</h1>
<ul>
{#todos}
	<li>{action} - {duedate}</li>{~n}
{/todos}
</ul>
```

And the resulting HTML:

```
<h1>My Todos</h1>
<ul>
  <li>learn some dust! - today</li>
  <li>visit the south bay - tomorrow</li>
</ul>
```

## More Information

Read more about DustJS at the LinkedIn Dust Tutorial: https://github.com/linkedin/dustjs/wiki/Dust-Tutorial




