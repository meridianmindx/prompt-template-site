# Chapter 6: F-Strings and Built-in Templating

## F-Strings as Template Engines

Python's f-strings offer a lightweight templating solution:

```python
name = "World"
message = f"Hello, {name}!"
```

## Limitations

- No conditional logic
- No loops
- No inheritance
- No filters

## When to Use F-Strings

- Simple substitutions
- Quick and dirty templating
- One-off messages

## Other Built-in Options

### str.format()

```python
template = "Hello, {name}!"
template.format(name="World")
```

### string.Template

```python
from string import Template

t = Template("Hello, $name!")
t.substitute(name="World")
```

### Textwrap

```python
import textwrap

template = """
Dear {name},
Your order #{id} has shipped.
Warm regards,
Team
"""

def render(template, **kwargs):
    for k, v in kwargs.items():
        template = template.replace(f"{{{k}}}", str(v))
    return template
```

## Comparison

| Method | Conditionals | Loops | Inheritance | Filters |
|--------|-------------|-------|-------------|---------|
| f-strings | No | No | No | No |
| str.format | No | No | No | No |
| Template | No | No | No | No |
| Jinja2 | Yes | Yes | Yes | Yes |
| Mako | Yes | Yes | Yes | Yes |

## When to Choose Each

- **F-strings**: Simple, one-off messages
- **str.format**: When you need multiple substitutions
- **Template**: When you need named placeholders
- **Jinja2/Mako**: When you need full templating power
