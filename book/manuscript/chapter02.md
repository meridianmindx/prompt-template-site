# Chapter 2: The Evolution of Text Templating in Python

## The Early Days: String Formatting

Before template engines existed, Python developers relied on basic string formatting:

```python
# Python 2.x / early 3.x
name = "World"
message = "Hello, %s!" % name
message = "Hello, {}".format(name)
```

This works for simple cases but breaks down as templates grow more complex.

## The First Wave: Genshi and Mako (2006-2009)

Genshi (2006) introduced Python-like expressions in templates with XML/HTML stream processing. Mako (2008) added a Python-like expression language embedded in templates, creating a new paradigm.

Key insight: templates should understand control flow, not just variable substitution.

## The Golden Age: Jinja2 (2009-Present)

Jinja2, inspired by Django's template system, became the de facto standard. Its design philosophy centered on:

- Sandboxed template execution
- Template inheritance
- Extensible filters and tests
- Automatic escaping
- Template-level programming (conditionals, loops, includes)

Flask adopted Jinja2 as its default template engine, cementing its position.

## The Modern Era: AI/Prompt Templating

With the rise of LLMs, a new class of template engines emerged. These are designed for constructing prompts with dynamic context, structured variables, and conversation history.

This book covers both traditional and modern template engines.

## The Template Engine Landscape Today

| Engine | Year | Domain | Template Language | Security Model |
|--------|------|--------|-------------------|----------------|
| Mako | 2008 | Web/General | Python-like expressions | Sandboxed |
| Jinja2 | 2009 | Web/General | Jinja2 syntax | Sandboxed |
| Chameleon | 2010 | Zope ecosystem | TAL/TALES | Sandboxed |
| Cheetah | 2001 | Web/General | Cheetah syntax | Sandboxed |
| Jinja3 | 2022+ | Web/General | Jinja2 syntax (improved) | Sandboxed |
| prompt-template-engine | 2025 | AI/LLM | Structured prompts | No sandbox (context injection) |

## What Every Developer Should Know

Regardless of which template engine you use, certain principles apply:

1. **Never trust template data** — always escape output
2. **Cache compiled templates** — avoid repeated parsing
3. **Keep logic out of templates** — use filters and helpers
4. **Test templates** — they're code, not markup
5. **Precompile** — especially in web frameworks

These principles appear throughout this book.
