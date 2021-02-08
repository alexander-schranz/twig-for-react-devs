# Twig for react devs

An easy guide for react developers getting into PHP Twig template engine from Symfony.

## Introduction

Hello 👋,

My name is Alexander Schranz ([alex_s_](https://twitter.com/alex_s_)) and I'm fulltime Webdeveloper
working on the [SULU CMS](https://sulu.io/?utm_source=github&utm_medium=repository&utm_campaign=alex-twig-for-react-devs)
and did based on that created a lot of websites.

With this article I wanted to make it easier for React Developers
getting into the Twig Syntax.

Every Section will first Provide the `React JS` ⚛️ code and then the `Twig` 🌱
followed by explanation about the difference between them.

**Table of Contents**

 - [Outputting a variable](#outputting-a-variable)
 - [Outputting raw HTML](#outputting-raw-html)
 - [If statements](#if-statements)
 - [Loops](#loops)
 - [Base Template](#base-template)
 - [Providing props to templates](#providing-props-to-templates)
 - [Include and override parts](#include-and-override-parts)
 - [Conclusion](#conclusion)

## Outputting a variable

In the first example we will to the most simpliest thing just
outputting a variable we did create in our component / template.

### Outputting a variable (React JS ⚛️)

```js
// src/views/My.js

import React from 'react';

class My extends React.Component {
    render() {
        const title = 'My Title';
        
        return <h1>{title}</h1>;
    }
}
```

### Outputting a variable (Twig 🌱)

```twig
{# templates/pages/my.html.twig #}

{% set title = 'My Title' %}

<h1>{{ title }}</h1>
```

### Outputting a variable (Explanation)

Outputting a variable in Twig and JSX is very similar instead
of `{variable}` you use `{{ variable }}`. The spaces are optional
but recommended by  the [Twig coding standards](https://twig.symfony.com/doc/3.x/coding_standards.html).

## Outputting raw HTML

There are cases where you use CKEditor or other rich text editors 
to create content which will provide you raw HTML code you would
like to render in this case you want not that your variable you
want to outputted does get escaped.

### Outputting raw HTML (React JS ⚛️)

```js
// src/views/My.js

import React from 'react';

class My extends React.Component {
    render() {
        const someHtml = '<p>My Text editor content</p>';

        return <div dangerouslySetInnerHTML={{_html: someHtml}} />;
    }
}
```

### Outputting raw HTML (Twig 🌱)

```twig
{# templates/pages/my.html.twig #}

{% set someHtml = '<p>My Text editor content</p>' %}

<div>
    {{ someHtml|raw }}
</div>
```

### Outputting raw HTML (Explanation)

Both (`React JS` and `Twig`) escape variables by default so no HTML
injected without  a call of an additional prop or filter.

There are in Twig other ways to disable `autoescape` over a whole
content. See for this the [official Twig Docs](https://twig.symfony.com/doc/3.x/api.html#escaper-extension).

I personally recommend only using the `|raw` filter todo this kind
of things.

## If statements

### If statements (React JS ⚛️)

```js
// src/views/My.js

import React from 'react';

class My extends React.Component {
    render() {
        var title = 'Test' ;
        const list = ['Test', 'Test 2'];

        // Basic equal if statement
        if (title == 'test') {
            title = 'Basic equal if statement';
        // Not equal if statement
        } else if (title != 'test') {
            title = 'Not equal if statement';
        // Typesafe if statemenet
        } else if (title === false) {
            title = 'Typesafe if statemenet';
        // Typesafe not if statemenet
        } else if (title !== false) {
            title = 'Typesafe not if statemenet';
        // In array if statemenet
        } else if (list.includes(title)) {
            title = 'In array if statemenet';
        // Not in array if statemenet
        } else if (!list.includes(title)) {
            title = 'Not in array if statemenet';
        // Greater if statement
        } else if (title > 10) {
            title = 'Greater if statement';
        // And if statement
        } else if (title > 10 && title < 10) {
            title = 'And if statement';
        // Or if statement
        } else if (title > 10 || title != 0) {
            title = 'Or if statement';
        // Else if statement
        } else {
            title = 'Else if statement';
        }
        
        return <div>
            {title}
            
            {title ? title : 'Other'}
        </div>;
    }
}
```

### If statements (Twig 🌱)

```twig
{# templates/pages/my.html.twig #}

{% set title = 'Test' %}
{% set list = ['Test', 'Test 2'] %}

{# Basic equal if statement #}
{% if title == 'test' %}
    {% set title = 'Basic equal if statement' %}
{# Basic not equal if statement #}
{% elseif title != 'test' %}
    {% set title = 'Basic not equal if statement' %}
{# Typesafe if statement #}
{% elseif title same as(false) %} {# Very uncommon in Twig todo typesafe checks #}
    {% set title = 'Typesafe if statement' %}
{# Typesafe not if statement #}
{% elseif title not same as(false) %} {# Very uncommon in Twig todo typesafe checks #}
    {% set title = 'Typesafe not if statement' %}
{# In array if statement #}
{% elseif title in list %}
    {% set title = 'In array if statement' %}
{# Not in array if statement #}
{% elseif title not in list %}
    {% set title = 'Not in array if statement' %}
{# Greater if statement #}
{% elseif title > 10 %}
    {% set title = 'Greater if statement' %}
{# And if statement #}
{% elseif title > 10 and title < 10 %}
    {% set title = 'And if statement' %}
{# Or if statement #}
{% elseif title > 10 or title != 0 %}
    {% set title = 'Or if statement' %}
{# Else if statement #}
{% else %}
    {% set title = 'Else if statement' %}
{% endif %}

<div>
    {{ title }}
    
    {{ title ?: 'Other' }}
</div>
```

### If statements (Explanation)

For if statement the [if tag](https://twig.symfony.com/doc/3.x/tags/if.html)
is used. Where in JavaScript it is very common to have
typesafe check with `===`. This is very uncommon in twig
because Twig is really focused being a template engine and
not a language where you should build business logic.

It is still possible in Twig doing a typesafe check using 
the [same as test](https://twig.symfony.com/doc/3.x/tests/sameas.html).

Twig like PHP supports also the ternary operator (`?:`) and the
null-coalescing operator (`??`). Read more about them in the
[Twig operators documentation](https://twig.symfony.com/doc/3.x/templates.html#other-operators).

## Loops

### Loops (React JS ⚛️)

```js
// src/views/My.js

import React from 'react';

class My extends React.Component {
    render() {
        const list = [{
            title: 'List Item 1',
        }, {
            title: 'List Item 2',
        }];
        
        return <ul>
            {list.map((item) => {
                return <li>
                    {item.title}
                </li>;
            })}
        </ul>;
    }
}
```

### Loops (Twig 🌱)

```twig
{# templates/pages/my.html.twig #}

{% set list = [
    {
        title: 'List Item 1',
    },
    {
        title: 'List Item 2',
    }
] %}

<ul>
    {% for item in list %}
        <li>{{ item.title }}</li>
    {% endfor %}
</ul>
```

### Loops (Explanation)

Where in `JavaScript` different way of looping exist in twig
you will work with the [for](https://twig.symfony.com/doc/3.x/tags/for.html)
to loop over variables.

Twig here supports some range function to make developing of
templates easier for example:

```twig
{# Output numbers from 0 - 10 #}
{% for i in 0..10 %}
    * {{ i }}
{% endfor %}

{# Output letters from a-z #}
{% for i in 'a'..'z' %}
    * {{ i }}
{% endfor %}

{# Output letters from A-Z #}
{% for letter in 'a'|upper..'z'|upper %}
    * {{ letter }}
{% endfor %}
```

By default inside the `for` loop Twig provide some magic variable
called `loop` which will provide you the following data:

```twig
{% for i in 0..10 %}
    {{ loop.index }} {# index beginning with 1 #}
    {{ loop.index0 }} {# index beginning with 0 #}
    {{ loop.revindex }} {# reverted index #}
    {{ loop.revindex0 }} {# reverted index0 #}
    {{ loop.first }} {# boolean flag if its the first item of this loop #}
    {{ loop.last }} {# boolean flag if its the last item of this loop #}
    {{ loop.length }} {# the number of items of this loop #}
    {{ loop.parent }} {# access the parent loop variable if using nested loops #}
{% endfor %}
```

Where in `React JS` you maybe need a `counter` or accessing the length
in `Twig` the `loop` variable is available inside every loop and make
it easy to add CSS classes to first or last items of a loop.

See here also the Twig documentation about the [for](https://twig.symfony.com/doc/3.x/tags/for.html)
tag.

As `React JS` has access to all `JavaScript` array available function
Twig also has it tricks to make outputting of html very simple.

So for example let see that we have variable which content is the
following:

```js
["1", "2", "3", "4", "5"]
```

and we want to render the following HTML:

```html
<div>
    <div class="row">
        <div class="col-4">
            1
        </div>

        <div class="col-4">
            2
        </div>

        <div class="col-4">
            3
        </div>
    </div>
    
    <div class="row">
        <div class="col-4">
            4
        </div>

        <div class="col-4">
            5
        </div>

        <div class="col-4">
            
        </div>
    </div>
</div>
```

This can in Twig easily be rendered by the `batch` function
the following way:

```twig
<div>
    {% for row in list|batch(3, '') %}
        <div class="row">
            {% for item in row %}
                <div class="col-4">
                    {{ item }}
                </div>
            {% endfor %}
        </div>
    {% endfor %}
</div>
```

Read here also the offical documentation about the
[Twig batch function](https://twig.symfony.com/doc/3.x/filters/batch.html).

## Base Template

The first you mostly create when building a website is a base
template or base component which contains all things which your
templates have in common like Navigation, Footer, Header.

### Base Template (React JS ⚛️)

In react for rendering anything we first need to create a
`index.html` where the react application is started which also
contains the minimal required html for a page.

```html
<!-- src/index.html --->
<!doctype html>
<html lang="en">
<head>

</head>
<body id="app">
    <script src="app.js"></script>
</body>
</html>
```

In react a base template could look like the following.

```js
// src/views/Base.js

import React from 'react';
import Head from 'next/head'
import Navigation from './components/Navigation';
import Header from './components/Header';
import Footer from './components/Footer';

class Base extends React.Component {
    render() {
        const {
            header
        } = this.props;
        
        const headerComponent = header ? header : <Header />;
        
        return <div>
            <Head>
                <title>{this.props.title}</title>
                <meta name="viewport" content="initial-scale=1.0, width=device-width" />
            </Head>
            
            <Navigation />
            
            <Header />
            
            <div className="content">
                {this.props.children}
            </div>

            <Footer />
        </div>;
    }
}
```

A default template build on top of the base template will look like the following way:

```js
// src/views/Default.js

import React from 'react';
import Base from './components/Base';

class Default extends React.Component {
    render() {
        const title = 'Some Title';

        return <Base title={title}>
            <h1>{title}</h1>
        </Base>;
    }
}
```

A homepage template build on top of the base template could look this way
which will override the Header component:

```js
// src/views/Homepage.js

import React from 'react';
import Base from './components/Base';
import HeaderBig from './components/HeaderBig';

class Homepage extends React.Component {
    render() {
        const title = 'Some Title';

        return <Base title={title}
                     header={<HeaderBig />}>
            <h1>{title}</h1>
        </Base>;
    }
}
```

This way we created a reusable base which can be used in all components again.

### Base Template (Twig 🌱)

To implement the same as we did above we need to create the following
in our Twig structure. As Twig is server rendered by PHP in Symfony
we will create the `base.html.twig` which will contain the html which
was in react rendered by `index.html` and `Base.js`.

```twig
{# templates/base.html.twig #}
<!doctype html>
<head lang="en">
    <title>{title}</title>
    <meta name="viewport" content="initial-scale=1.0, width=device-width" />
</head>
<body>
    {% include 'include/navigation.html.twig' %}
    
    {% block header %}
        {% include 'include/header.html.twig' %}
    {% endblock %}
    
    {% block content %}{% endblock %}
    
    {% include 'include/footer.html.twig' %}
</body>
</html>
```

And now we create the 2 templates file which use this `base.html.twig` file.

```twig
{# templates/pages/default.html.twig #}
{% extends 'base.html.twig' %}

{% block content %}
    <h1>{title}</h1>
{% endblock %}
```

We will also create the `homepage.html.twig` with a custom header.

```twig
{# templates/pages/homepage.html.twig #}
{% extends 'base.html.twig' %}

{% block header %}
    {% include 'include/header-big.html.twig' %}
{% endblock %}

{% block content %}
    <h1>{title}</h1>
{% endblock %}
```

### Base Template (Explanation)

As you see in this example the `extends` will say from which template
they should inherit and with the `block` feature of Twig we can then
provide content or override the defined `blocks` like `header` and
`content` in our Twig template.

Instead of importing a components like its done react we will use
in Twig the `include` block/method of Twig to render the content of another twig
template.

Has in this case a look at the [extends tag documentation](https://twig.symfony.com/doc/3.x/tags/extends.html)
and [include tag documentation](https://twig.symfony.com/doc/3.x/tags/include.html)
from the official [Twig Documentation](https://twig.symfony.com/).

## Providing props to templates

### Providing props to templates (React JS ⚛️)

```js
// src/views/Parent.js

import React from 'react';
import Child from './components/Child';

class Parent extends React.Component {
    render() {
        const someTitle = 'Some Title';

        return <Child title={someTitle} />;
    }
}
```

### Providing props to templates (Twig 🌱)

```twig
{% set title = 'Some Title' %}

{% include 'includes/child.html.twig' %}
```

### Providing props to templates (Explanation)

Where in `React JS` the only way to give properties from a parent
to a child is giving them as a `props` in `Twig` by default a
included `Template` has access to all variables which were defined
or are accessible in the parent template.

Twig still provide ways to provide properties to included template
this way:

```twig
{% include 'includes/child.html.twig' with { title: 'Some Title' } %}
```

If you don't want that the included template can access the variables
defined in the parent template this can be avoided using the `only`
keyword:

```twig
{% set someTtile = 'Some Title' %}

{% include 'includes/child.html.twig' with { title: someTitle } only %}
```

Read more about the include tag in the official [Twig Include Documentation](https://twig.symfony.com/doc/3.x/tags/include.html).

## Include and override parts

Coming soon ...

### Include and override parts (React JS ⚛️)

```js
// src/views/Parent.js

import React from 'react';
import Child from './components/Child';
import CustomHeader from './components/CustomHeader';
import CustomFooter from './components/CustomFooter';

class Parent extends React.Component {
    render() {
        return <Child
                header={<CustomHeader />}
                footer={<CustomFooter />}>
            {this.props.children}
        </Child>;
    }
}
```

```js
// src/component/Child.js

import React from 'react';
import Child from './components/Child';
import Header from './components/Header';
import Footer from './components/Footer';

class Parent extends React.Component {
    render() {
        const HeaderComponent = this.props.header ? this.props.header : <Header />;
        const FooterComponent = this.props.footer ? this.props.footer : <Footer />;
        
        return <div>
            {HeaderComponent}
            {FooterComponent}
        </div>;
    }
}
```

### Include and override parts (Twig 🌱) (using include)

```twig
{# templates/pages/parent.html.twig #}

{% include 'includes/other-child.html.twig' %}
```

```twig
{# templates/pages/other-child.html.twig #}

{% extends 'includes/child.html.twig' %}

{% block header %}
    {% include 'includes/custom-header.html.twig' %}
{% endblock %}

{% block content %}
    Some Content
{% endblock %}

{% block footer %}
    {% include 'includes/custom-footer.html.twig' %}
{% endblock %}
```

```twig
{# templates/pages/child.html.twig #}

{% block header %}
    {% include 'includes/header.html.twig' %}
{% endblock %}

{% block content %}
    
{% endblock %}

{% block footer %}
    {% include 'includes/footer.html.twig' %}
{% endblock %}
```

### Include and override parts (Twig 🌱) (using embed)

```twig
{# templates/pages/parent.html.twig #}

{% embed 'includes/child.html.twig' %}
    {% block header %}
        {% include 'includes/custom-header.html.twig' %}
    {% endblock %}
    
    {% block content %}
        Some Content
    {% endblock %}
    
    {% block footer %}
        {% include 'includes/custom-footer.html.twig' %}
    {% endblock %}
{% endembed %}
```

```twig
{# templates/pages/child.html.twig #}

{% block header %}
    {% include 'includes/header.html.twig' %}
{% endblock %}

{% block content %}
    
{% endblock %}

{% block footer %}
    {% include 'includes/footer.html.twig' %}
{% endblock %}
```

### Include and override parts (Explanation)

In Twig there are different solutions for this kind of task.
Where mostly you would work in Twig with `extends` from a
specific `base.html.twig` and maybe have 1-3 different kind
of this base template, its very uncommon in Twig that you
include a component template and override a part of it.

Mostly Twig devs will go here with a additional Twig template
and use `extends` and in the other template an `include` tag
to the new template.

Twig provide with the `embed` tag here to combine a `include`
and `extends` in one template and avoid so the additional
template.

As this pattern is very uncommon you should not overuse the
`embed` tag as it make the templates unreadable.

## Conclusion

Twig and React JS (JSX) are in some cases very similar how they
render and output things. Twig is really focused of just creating
HTML or the outputted file format, like what `JSX` was designed for.

Where in `React JS` you have access to all JavaScript functions,
`Twig` is very strict here as it is designed that all your Business Logic
lives in your PHP code before Twig is callded.
So if your Twig code begins to look messy or you have a lot of logic
statements in it, it is a hint that you did accidentally move your 
Business Logic which should be handled in your PHP Controller
into your template rendering, which should be avoided.

I hope with this article I can give React Developers an easier
way to get into Twig.

For getting an overview of all available Twig functionality I recommend
to read over the [Official Twig Documentation](https://twig.symfony.com/).

Thank you for reading this!

More articles on Github from me can be found [here](https://github.com/topics/alexander-schranz-article).
