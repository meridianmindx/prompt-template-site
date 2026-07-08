# Chapter 5: Chameleon and Genshima — The Zope Family

## Overview

Chameleon is a high-performance HTML templating engine for Zope, inspired by Cheetah and Genshama.

## Core Concepts

### TAL Templates

```python
from chameleon import PageTemplate
from chameleon.zpt import PageTemplateFile

# Basic template
template = PageTemplate("<div>Hello, ${name}!</div>")
print(template(name="World"))
```

### Data Access (TAL)

```python
# Template: <div tal:content="name">Default</div>
# Data: {"name": "World"}
```

### Attributes and Loops

```html
<ul>
  <li tal:repeat="item items">
    ${item.name}
  </li>
</ul>
```

## Performance

Chameleon compiles templates to Python code, making it one of the fastest options.

## Integration

Chameleon is the default for Zope 3. It supports:

- Template inheritance
- Conditional rendering
- Attribute binding
- Expression language

## When to Use

- Zope ecosystem projects
- High-performance web apps
- HTML-specific templating
