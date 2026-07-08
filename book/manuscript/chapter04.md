# Chapter 4: Mako - Speed and Flexibility

## Overview

Mako, created by Mike Bayer (creator of SQLAlchemy), offers Python-like expression support with extreme performance.

## Core Concepts

### Expressions

```python
from mako.template import Template

t = Template("Hello ${name}")
print(t.render(name="World"))
```

### Control Structures

```python
from mako.template import Template

t = Template("""
% for item in items:
  - ${item}
% endfor
""")

print(t.render(items=["a", "b", "c"]))
```

### Inheritance

```mako
%inherit name="base.mako"

%block title
Child Title
%endblock
```

### Imports and Functions

```mako
%import mako.template

%def name
My Template
%enddef

<header>
  ${header}
</header>
```

## Performance

Mako compiles templates to Python bytecode, making it significantly faster than Jinja2 for complex templates.

```python
from mako.template import Template

# Precompile for production
compiled = Template("Hello ${name}", compiledir="./compiled")
print(compiled.render(name="World"))
```

## Advanced Features

### Catch Block

```python
from mako.template import Template

t = Template("""
%try:
  ${bad_var}
%except:
  <p>Error</p>
%endtry
""")
```

### Filters

```python
from mako.template import Template

t = Template("<div>${name | h}</div>")
print(t.render(name="<script>"))
# Output: <div>&lt;script&gt;</div>
```

## When to Use Mako

- High-performance applications
- Complex template logic
- When you need Python-like expressions
- When you want bytecode compilation

## When Not to Use Mako

- When you need Jinja2's ecosystem
- When template designers aren't Python developers
- When you need template inheritance with multiple levels
