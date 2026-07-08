# Chapter 3: Jinja2 - The Industry Standard

## Overview

Jinja2 is Python's most widely used template engine, with 6M+ downloads. It powers Flask, the most popular Python web framework.

## Core Concepts

### Variables and Expressions

```python
from jinja2 import Template

template = Template("Hello, {{ name }}!")
print(template.render(name="World"))
```

### Control Structures

```python
from jinja2 import Template

template = Template("""
{% for item in items %}
  - {{ item }}
{% endfor %}
""")

print(template.render(items=["a", "b", "c"]))
```

### Template Inheritance

```jinja2
{# base.html #}
<html>
  <head><title>{% block title %}Default{% endblock %}</title>
</html>

{# child.html #}
{% extends "base.html" %}
{% block title %}Child Page{% endblock %}
```

### Filters and Tests

```python
from jinja2 import Environment

env = Environment()
env.filters['reverse_string'] = lambda s: s[::-1]
env.tests['is_long'] = lambda x: len(x) > 5
```

### Macros

```jinja2
{% macro button(text, href) %}
  <a href="{{ href }}" class="btn">{{ text }}</a>
{% endmacro %}

{{ button("Click", "/path") }}
```

## Advanced Jinja2

### Context Classes

```python
from jinja2 import Environment

class MyContext:
    name = "World"
    
    def greet(self):
        return "Hello, " + self.name

env = Environment()
template = env.from_string("{{ ctx.name }}")
ctx = MyContext()
print(template.render(ctx=ctx))
```

### Custom Tag Blocks

```python
from jinja2 import Environment, TemplateSyntaxError

env = Environment()
template = env.from_string("{{ super() }}")
```

## Performance Tips

1. **Cache compiled templates** — compile once, render many times
2. **Precompile templates** — avoid parse overhead
3. **Use `Template()` directly** — skip the environment for simple use cases

```python
from jinja2 import Template

# BAD: re-creates template each time
# template = Template("{{ name }}")

# GOOD: cache
TEMPLATES = {}
def get_template(key):
    if key not in TEMPLATES:
        TEMPLATES[key] = Template(TEMPLATES[key])
    return TEMPLATES[key]
```

## Security

Jinja2 runs in a sandboxed environment, but:

- Never pass untrusted templates (template injection)
- Use `autoescape=True` for HTML output
- Use `BaseException`-based security

```python
from jinja2 import Environment, FileSystemLoader, ChoiceLoader

# Safe: limit to specific directories
env = Environment(loader=FileSystemLoader('templates'))

# Safe: restrict to one directory
env = Environment(loader=ChoiceLoader([
    FileSystemLoader('templates'),
]))
```

## Real-World Patterns

### Conditional Blocks

```jinja2
{% if user.is_admin %}
  <p>Welcome, Admin!</p>
{% else %}
  <p>Welcome, User!</p>
{% endif %}
```

### Including Templates

```jinja2
{% include "header.html" %}
{% include "footer.html" %}
```

### Loop with Conditions

```jinja2
{% for item in items if item.active %}
  <li>{{ item.name }}</li>
{% endfor %}
```

## Transition to Modern Templating

Jinja2 is powerful for text templating, but modern AI workflows need structured prompt templating. The next chapter explores this evolution.
