# Chapter 8: Advanced Patterns - Inheritance, Filters, and Macros

## Template Inheritance: Building on a Foundation

Template inheritance lets you define a base template with blocks that children override. This is the most powerful feature of modern template engines.

```jinja2
{# base.html - the foundation #}
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Default Title{% endblock %}</title>
    {% block head %}{% endblock %}
</head>
<body>
    {% block header %}
        <header>Default Header</header>
    {% endblock %}
    <main>
        {% block content %}
            <p>Default content</p>
        {% endblock %}
    </main>
    {% block footer %}
        <footer>&copy; 2026</footer>
    {% endblock %}
</body>
</html>

{# child.html - overrides blocks #}
{% extends "base.html" %}
{% block title %}About Page{% endblock %}
{% block content %}
    <h1>About Us</h1>
    <p>We are a template engine company.</p>
{% endblock %}
```

## Inheritance with Multiple Levels

```jinja2
{# grandchild.html - overrides a child's block #}
{% extends "child.html" %}
{% block content %}
    <h1>Grandchild Page</h1>
    <p>This overrides child's content block.</p>
{% endblock %}
```

## Custom Filters

Filters transform template output:

```python
from jinja2 import Environment

env = Environment()

def markdown(text):
    """Convert simple markdown-like text."""
    return text.replace("**", "<b>").replace("*", "</b>")

env.filters['markdown'] = markdown
env.filters['uppercase'] = lambda s: s.upper()
env.filters['truncate'] = lambda s, n: s[:n] + '...' if len(s) > n else s
```

## Custom Tests

Tests evaluate conditions in templates:

```python
from jinja2 import Environment

env = Environment()
env.tests['is_valid_email'] = lambda email: '@' in email and '.' in email

# In template:
# {% if email is is_valid_email %}
```

## Macros: Templates as Functions

Macros are reusable blocks of template code:

```jinja2
{% macro render_button(text, url) %}
    <a href="{{ url }}" class="btn btn-primary">{{ text }}</a>
{% endmacro %}

{% macro render_link(text, url) %}
    <a href="{{ url }}" class="link">{{ text }}</a>
{% endmacro %}

{{ render_button("Click Me", "/action") }}
```

## Importing Macros

```jinja2
{% from "buttons.html" import render_button, render_link %}
{{ render_button("Click", "/action") }}
```

## Block Inheritance with super()

```jinja2
{% extends "base.html" %}
{% block content %}
    <p>Before content</p>
    {{ super() }}  {# Includes parent's content #}
    <p>After content</p>
{% endblock %}
```

## Context Managers in Templates

```jinja2
{% with total = items|length %}
    <p>Found {{ total }} items</p>
{% endwith %}
```

## Template Composition Patterns

### Mixins

```jinja2
{# mixins/alert.html #}
{% macro alert(type, message) %}
    <div class="alert alert-{{ type }}">
        {{ message }}
    </div>
{% endmacro %}
```

### Slots and Placeholders

```jinja2
{# In a page template #}
{% extends "layout.html" %}
{% block sidebar %}
    <ul>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</li>
    </ul>
{% endblock %}
```

## Advanced Jinja2 Features

### Loop Variables

```jinja2
{% for item in items %}
    {{ loop.index }}. {{ item }}
{% endfor %}
```

Available loop variables:
- `loop.index` - 1-based index
- `loop.index0` - 0-based index
- `loop.first` - True if first iteration
- `loop.last` - True if last iteration
- `loop.length` - Total count

### Conditional Filters

```jinja2
{{ name|default("Guest", true) }}
{{ text|truncate(100, true) }}
{{ price|currency("USD") }}
```

### Template Inheritance Best Practices

1. **Start with a base template** - Define the structure
2. **Use block tags for sections** - Title, head, body, sidebar, footer
3. **Use super() to extend** - Don't completely override when you can extend
4. **Keep it simple** - Don't nest too deeply

### Macros for Complex Output

```jinja2
{# In template #}
{% macro render_profile(user) %}
    <div class="profile">
        <img src="{{ user.avatar }}" />
        <h2>{{ user.name }}</h2>
        <p>{{ user.bio }}</p>
    </div>
{% endmacro %}

{{ render_profile(current_user) }}
```

### Context Objects and Classes

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    @property
    def initial(self):
        return self.name[0].upper()
```

```jinja2
{{ user.initial }} - {{ user.name }}
```

## Performance Considerations

### Template Caching

```python
from jinja2 import Environment, FileSystemLoader

env = Environment(
    loader=FileSystemLoader("templates"),
    cache_size=100,  # Cache 100 compiled templates
)
```

### Precompilation

```python
from jinja2 import Environment

env = Environment(loader=FileSystemLoader("templates"))
for name in env.list_templates():
    env.get_template(name).render()
```

### Avoid Template String Parsing

```python
# BAD: Parse template every time
template = Template("Hello, {{ name }}!")
print(template.render(name="World"))

# GOOD: Parse once
template = Template("Hello, {{ name }}!")
print(template.render(name="World"))
```

## Security Best Practices

### Automatic Escaping

```python
env = Environment(autoescape=True)
```

### Safe Markup

```python
from markupsafe import Markup

safe_html = Markup("<strong>Bold</strong>")
```

### Avoid Template Injection

```python
# DANGEROUS: User-controlled template
Template(user_input).render()

# SAFE: Use a whitelist
ALLOWED_TEMPLATES = ["welcome", "about"]
if template_name in ALLOWED_TEMPLATES:
    Template(template_name).render()
```

## Advanced Mako Patterns

### Expressions and Conditionals

```python
from mako.template import Template

template = Template("""
% if name == 'admin':
    <p>Welcome, Admin!</p>
% else:
    <p>Welcome, ${name}!</p>
% endif
""")
```

### Filters and Encoding

```python
# Built-in filters
"name | h"           # HTML escape
"name | xml"        # XML escape
"name | n"          # No escaping
"name | url"        # URL encode
```

### Importing Modules

```mako
<%namespace name="utils" file="utils.mako" />
${utils.render_header()}
```

### Context Class

```python
class Context:
    def __init__(self):
        self.name = "World"
    
    def greet(self):
        return f"Hello, {self.name}!"
```

## Advanced Chameleon Patterns

### TAL Expressions

```python
from chameleon import PageTemplate

# Data
data = {"users": [{"name": "Alice"}, {"name": "Bob"}]}

# Template
template = PageTemplate("""
<div tal:repeat="user users">
    <span tal:content="user/name">User</span>
</div>
""")

print(template(users=data["users"]))
```

### Conditional Attributes

```python
template = PageTemplate("""
<a tal:attributes="href url" tal:condition="has_link">Link</a>
""")
```

## Comparing All Approaches

### Text Templating

Use Jinja2/Mako for:
- HTML pages
- Email templates
- Configuration files
- Code generation

### F-Strings

Use f-strings for:
- Simple variable substitution
- Quick, one-off messages

### Prompt Templating

Use prompt-template-engine for:
- LLM prompts
- Structured prompt building
- AI workflow orchestration

## Next: Security Considerations

In the next chapter, we'll cover security best practices for template engines.
