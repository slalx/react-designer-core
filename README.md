react-designer-core
=======================

Warning - it is prototype and work in progress.

React designer is WYSIWYG editor for **easy content creation** (legal contracts, business forms, marketing leaflets, technical guides, visual reports, rich dashboards, tutorials and other content, etc.).

[Live demo](http://rsamec.github.io/react-designer-core/)

It is based on separation between

+   logical component tree - JSON simple document description - [Page Transform Tree (PTT)](https://github.com/rsamec/ptt) - it is framework agnostic definition
+   visual component tree - [React virtual DOM](http://facebook.github.io/react) - rendering to DOM so that it maps each component (terminal node) from logical tree to react component and its properties

## Main goals 

+   it is aimed for developers - to use it in their own solutions  
+   it is free and open source
+   it is simple by design - minimal JSON framework agnostic definition maps directly to React components       

## Features

+   directly manipulate the layout of a document without having to type or remember names of components, elements, properties or other layout commands.
+   high-quality on-screen output and on-printer output
+   precise visual layout that corresponds to an existing paper version
    +   support for various output formats - html, pdf.
+   comfortable user experience - basic WYSIWYG features
    +   support drag nad drop - resize object length, move object positions
    +   support manipulating objects -> copy, move, up, down objects in object schema hierarchy
    +   highlighting currently selected object and its parent
+   build-in html content publishing (preview of html dynamic document)
+   binding support using [react-binding](https://github.com/rsamec/react-binding) - experimental
+   props inheritance - when rendering occurs -> the props value is resolved by using a value resolution strategy (Binding Value -> Local Value -> Style Value -> Default Value)
+   usable for big documents - careful designed to use react performance
    +   we won't render the component if it doesn't need it
    +   simple comparison is fast because of using immutable data structure
+   undo/redo functionality


It comes with core components typical for WYSIWYG designers

+   Workplace - main working area for drawing documents
+   PropertyGrid - component properties editors (html editors, color pickers, json editors, etc.)
+   ObjectTree - logical component tree - enables to search and move components between nodes


### Logical component tree - [PTT](https://github.com/rsamec/ptt)

The PPT format is __framework agnostic__ document description. It enables to dynamically render pages in different sizes (A4,A3,Letter,...) and in various formats (html,pdf).

It is a simple component tree that consists of these two nodes

+   **containers** - nodes that are invisible components - usable for logical grouping of reactive parts of document (sections)
+   **boxes** - terminal nodes (leaf) that are visible components - (components, boxes, widgets) - it maps to props of component

There is an minimal 'Hello world' example. The logical tree consists of one container and one box with TextBox element.

```json
{
 "name": "Hello World Example",
 "elementName": "PTTv1",
 "containers": [
    {
     "name": "container",
     "elementName": "Container",
     "style": { "top": 0, "left": 0, "height": 200, "width": 740, "position": "relative" }
     "boxes": [{
        "name": "TextBox",
        "elementName": "TextBox",
        "style": { "top": 0, "left": 0 },
        "props":{
             "content": "Hello world"
            }
        }],
    }]
}
```

See the full [PTT specification](https://github.com/rsamec/ptt).

### Visual componenet tree - using react

To render react components specifies in react is really simple

```js
    createComponent: function (box) {
        var widget =widgets[box.elementName];
        if (widget === undefined) return React.DOM.span(null,'Component ' + box.elementName + ' is not register among widgets.');

        return React.createElement(widget,box, box.content!== undefined?React.DOM.span(null, box.content):undefined);
    },
    render:function(){
       {this.props.boxes.map(function (box, i) {
                var component = this.createComponent(box);
                return (
                       <div style={box.style}>
                            {component}
                       </div>
                       );
       }, this)}
    }
```

## Demo & Examples

[Live demo](http://rsamec.github.io/react-designer-core/)

To build the examples locally, run:

```
npm install
gulp dev
```

Then open [`localhost:8000`](http://localhost:8000) in a browser.


## Installation

The easiest way to use this component is to install it from NPM and include it in your own React build process (using [Browserify](http://browserify.org), [Webpack](http://webpack.github.io/), etc).

You can also use the standalone build by including `dist/react-shapes.js` in your page. If you use this, make sure you have already included React, and it is available as a global variable.

```
npm install react-designer-core --save
```

## Usage

import {Workplace,ObjectBrowser,ObjectPropertyGrid} from 'react-designer-core';

See the example folder.

## Roadmap

+   support for typography (vertical rhythm, modular scale, web fonts, etc.)
+   improve designer experience
    +   move objects in object browser
    +   disabled add widget when box is selected
    +   improve property editor
+   performance issues
    +   recheck - should component update
    +   parse property values (parseInt,etc.) - to many places - remove defensive programming favor contract by design
+   data binding refactoring    
    +   support for binding to remote stores (consider falcor)
    +   data watchers - if some data changes, it changes on the other site
+   support for more positioning schemas - flex, grid, ...
+   better support for PDF
    +   better support html fragments -> to pdf (using html parser) - consider using pdfmake

### License

MIT. Copyright (c) 2015 Roman Samec
