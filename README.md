cosyJS
======

Full stack UI component library and layout generator

## Intro

**cosyJS is still experimental and should not be used for production environment**

The idea is to provide you out of the box a node.js server which manage all your static files (stylesheets, javascript templates, javascript lib,...).

cosyJS has also some nice features out of the box like:

* A layout generator using Bootstrap (and later Foundation and Cosy Grid Stystem)
* A powerfull client-side library which allows you to play easily with your HTML and/or the cosyJS components. 
* A library of full-stack components which has a set of UI element you use all the time (text, button, image).

## Try it and give feedback

This is published mainly to give the possibility to try cosy and give me feedback. Do not use this in a production environment. This is still experimental.

```javascript
npm install cosy
```

## Shared Templates

The idea behind cosy is to shared the Templates you use to render your data between client-side and server-side. cosyJS expect an Express (or at least connect) server.

## Work in progress

This is still currenly in development but you are free to collaborate on the project.

Here is a list of task you could do:

* Create new full-stack components
* Improve performance
* Share your opinions and comments

##Roll your own

Right now Cosy required an express instance in order to work, here is the code to add Cosy to your existing express instance.

```javascript
  var express = require('express'),
      http = require('http'),
	  app = express(),
	  cosy = require('cosy')(app);

  cosy.start(function(){
  	http.createServer(app).listen(app.get('port'), function() {
        console.log("Express server listening on port " + app.get('port'));
    });
  });
```
##The Component

By using node.js we are able to run the same code client and server side and cosy tries to take this to its advantage and allows you to share lots of code between them.

A component needs 2 things in order to work, the definition and the template.

**The definition**

It is there where you will put the logic of your component. This component is a wrapper for a Backbone.Model anb a Backbone.View. So you can use all the backbone syntax in it.

A basic component definion:

```javascript
_c.component({
    name: "basicComponent",
    events: {
    	"click .someClass": "doSomething"
    },
    doSomething: function() {
    	alert("hello world")
	}
});
```

**The template**

The template uses handlbar (client/server) syntax to produce the HTML.

Here is a template for the "basicComponent" above:

```html
<div>
  <button class="someClass">Do Something</button>
</div>
```
##The layout

When you have some components, you now need to add them to a page. But a page needs a layout.
The layout is mainly used to define some custom stylesheets, some script (GA),...

Layout uses the JADE notation.

Here is a really simple layout:

```jade
doctype 5
  html
	head
	    title
	    link(href="http://twitter.github.io/bootstrap/assets/css/bootstrap.css", rel="stylesheet")
	    link(href="/stylesheets/style.css", rel="stylesheet")
	  body
	    div.container
	        |<%=content%>
	    script
	        window.__c_conf = <%= conf %>;
	    script(src="http://code.jquery.com/jquery-2.0.0.js")
	    script(src="/javascripts/c.js")
```

The required part of this layout is the 3 last lines.

NOTE: Please note, we are trying to improve this part to use our other module called "Hygge" (a script file loader). More information will come.

##The Page Structure

You page will also need a page structure, this page structure could be reuse by multiple pages.

Here is an example of a Page Structure:

```javascript
 var basicStructure = 
 {  
  name:     "root",
  order: 0,
  columns:  [
              {
                name:     "header",
                columns:  [
                            { name: "logo", size:"4", parent: "header", order: 0 },
                            { name: "menu", size:"8", parent: "header", order: 1 },
                          ],
                parent: "root",
                order: 0
              },
              {
                name:     "content",
                columns:  [
                            { name: "main", size:"12", parent: "content", order: 0 }                        
                          ],
                parent: "root",
                order: 1
              },
              {
                name:     "footer",
                columns:  [
                            { name: "footerContent", size:"12", parent: "footer", order: 0 }                            
                          ],
                parent: "root",
                order: 2
              }
  ]
};
```

The page structure defines the placeholders you want to add in your page.

You could use cosy without a CSS grid but cosy assumes you are using one, and let you define the size of the Column you want.

##The Page

The page contains all the component information and where the component are located for a specific route.

Here is an example:

```javascript
  {
    name: "index",
    layout: "layout",    
    route: "/",
    components: [
      { type: "text", category: "h1", id: "Logo", order:1 , dynamic: true, placeholder: "logo", data: { text: "Cosy Js" } },
      { type: "image", source: "https://redappleapartments.files.wordpress.com/2012/05/copenhagen1.jpg", id: "image", order:4, placeholder: "main" },
    ]
  }
```

As you can see, the page is defined by a name (title of the Page), a route (the URL in your adress bar), the layout (he needs to use) and a list of components. Each component needs to have the name of the placeholder you want to insert itself and a data object. This data will be used by the component.


##Put it all together

What cosyJS does under the hood is to assembly all these static definitions to produce a dynamic website.

##Dealing with Data

As you can see, there is nothing related to Data communcation, cosy lets you defined how and where you want to fetch your Data. The idea is to extend the APP object and to create a REST API using express. Hence, in your component, you will be able to fetch the data you want.

TODO: We will try to find an easy way for you to work "out of the box" with a REST Api.

##Conclusion

As you can see, this is an ambitious project and currently you are just able to produced really basic website with it but we are working hard to enhance this and you are welcome to contribute to the project.