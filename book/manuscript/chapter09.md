# Chapter 9: Security Considerations in Template Engines

## The Security Model

Template engines provide a sandboxed environment for template execution. This prevents arbitrary code execution in templates.

### Jinja2 Security

Jinja2 templates execute in a sandboxed environment, but:

- Templates cannot access Python objects directly
- Templates cannot call Python methods
- Templates cannot modify Python state

```python
from jinja2 import Environment, FileSystemLoader, ChoiceLoader

# Safe: Limit template loading to specific directories
env = Environment(loader=ChoiceLoader([
    FileSystemLoader('templates'),
]))
```

### Mako Security

Mako compiles templates to Python bytecode, which can execute arbitrary code. Be careful about template source:

```python
from mako.template import Template

# Safe: Only compile templates from trusted sources
template = Template("Hello ${name}")
print(template.render(name="World"))
```

### Chameleon Security

Chameleon uses TAL (Template Attribute Language) which is a safe, declarative syntax:

```python
from chameleon import PageTemplate

# Safe: TAL expressions are evaluated in a restricted context
template = PageTemplate("<div>Hello, ${name}!</div>")
```

## Input Validation

Always validate input data before passing it to templates:

```python
from jinja2 import Environment

env = Environment()

# Validate before passing to template
if not name.isalnum():
    raise ValueError("Invalid name")

template = env.from_string("Hello, {{ name }}!")
print(template.render(name=name))
```

## Output Escaping

Escape output to prevent XSS:

```python
from jinja2 import Environment

env = Environment(autoescape=True)
template = env.from_string("{{ content }}")
print(template.render(content="<script>alert('xss')</script>"))
# Output: &lt;script&gt;alert('xss')&lt;/script&gt;
```

## Template Injection Prevention

Prevent template injection attacks:

```python
ALLOWED_TEMPLATES = {"welcome", "about", "contact"}

template_name = request.args.get("template")
if template_name in ALLOWED_TEMPLATES:
    env.get_template(template_name).render()
```

## Cross-Site Scripting (XSS)

Always escape user input in templates:

```python
# Jinja2 with autoescape
env = Environment(autoescape=True)

# Or manually escape
from markupsafe import Markup
safe_html = Markup("<strong>Bold</strong>")
```

## Content Security Policy

Use Content Security Policy headers to prevent XSS:

```python
@app.route("/")
def index():
    return render_template("index.html"), 200, {
        "Content-Security-Policy": "default-src 'self'"
    }
```

## Template Inheritance Attacks

Ensure templates are properly isolated:

```python
# Don't allow user-controlled template names
# BAD
Template(user_input).render()

# GOOD: Whitelist template names
TEMPLATES = {
    "welcome": "Welcome, {{ name }}!",
    "about": "About Us"
}

if template_name in TEMPLATES:
    print(Templates[template_name])
```

## Error Handling

Handle errors gracefully:

```python
from mako.template import Template
from mako.exceptions import TemplateSyntaxError

try:
    template = Template("Hello ${name}")
except TemplateSyntaxError as e:
    print(f"Template error: {e}")
```

## Caching Strategies

Cache compiled templates to avoid repeated parsing:

```python
TEMPLATE_CACHE = {}

def get_template(name):
    if name not in TEMPLATE_CACHE:
        TEMPLATE_CACHE[name] = Template(USERS[template_name])
    return TEMPLATE_CACHE[name]
```

## Best Practices

1. **Always escape output** - Prevent XSS attacks
2. **Validate input** - Don't pass untrusted data to templates
3. **Limit template sources** - Only allow templates from trusted locations
4. **Use autoescaping** - Enable automatic HTML escaping
5. **Precompile templates** - Avoid parsing overhead
6. **Use template inheritance** - Keep templates DRY

## Advanced Security Features

### Jinja2 Security with Restricted Context

```python
from jinja2 import Environment, FileSystemLoader

env = Environment(
    loader=FileSystemLoader("templates"),
    autoescape=True,
)

# Only allow specific templates
RESTRICTED = {"welcome", "about", "contact"}

for name in RESTRICTED:
    env.get_template(name).render()
```

### Mako Safe Mode

```python
from mako.template import Template

# Mako has safe mode by default
template = Template("Hello ${name}")
print(template.render(name="World"))
```

### Chameleon Security

```python
from chameleon import PageTemplate

# Chameleon is safe by design
template = PageTemplate("<div>Hello, ${name}!</div>")
```

## Testing for Security

Test templates for vulnerabilities:

```python
# Test for XSS
template = Template("Hello ${name}")
print(template.render(name="<script>alert('xss')</script>"))
# Should output: Hello <script>alert('xss')</script>

# Test for template injection
# Should reject: {{7*7}}
```

## Conclusion

Template engines provide a secure environment for template execution. By following best practices, you can prevent XSS attacks, template injection, and other security vulnerabilities.

In the next chapter, we'll cover performance and caching strategies.
