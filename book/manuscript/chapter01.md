# Chapter 1: Introduction to Template Engines

## What is a Template Engine?

A template engine is a software library that separates the presentation layer from the application logic. Templates are pre-designed documents that contain placeholders for dynamic content. When data is provided, the engine fills in the blanks to produce a final output.

Template engines serve several critical purposes:

1. **Separation of concerns** — Templates handle the "how it looks," code handles the "how it works"
2. **Reusability** — Write a template once, apply it to many different datasets
3. **Accessibility** — Non-programmers can edit templates without touching application code
4. **Performance** — Compiled templates execute faster than interpreted code

## Where Template Engines Shine

Template engines are used across many domains:

- **Web development** — HTML generation (Flask uses Jinja2 as default)
- **Configuration files** — Environment-specific YAML/JSON with variables
- **Email templates** — Personalized messages with dynamic content
- **Report generation** — Structured documents with computed values
- **Code generation** — Scaffold files with project-specific names and paths
- **AI/LLM workflows** — Structured prompts for large language models

## The Two Main Families

There are two fundamentally different families of template engines:

### Text Templating (Traditional)

These engines generate text output from templates with placeholders. They're mature, well-understood, and battle-tested across decades of production use.

Examples: Jinja2, Mako, Chameleon, Cheetah

### Prompt Templating (Modern/AI)

These engines structure prompts for AI/LLM workflows. They use structured variables, system/user templates, and dynamic context injection.

Examples: prompt-template-engine, LangChain, Databricks

## Why This Book Matters

Python's template engine ecosystem spans over two decades of evolution. Understanding each generation helps you make better choices about which tool to use and when. This book takes you from basic templating through to modern AI-driven template workflows.

By the end of this book, you'll be able to:

- Choose the right template engine for each use case
- Write secure, performant templates
- Build custom template engines when needed
- Integrate templates into production systems
