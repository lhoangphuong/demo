# Frontend skill

Use this skill when adding or modifying frontend code in this Flask app.

## Stack

- Templates live in `templates/` (Jinja2, served by Flask)
- Static assets live in `static/` (CSS in `static/css/`, JS in `static/js/`)
- No build step — plain HTML, CSS, and vanilla JS only
- Use `url_for('static', filename='...')` for all asset references in templates

## Adding a new page

1. Create `templates/<name>.html` extending `templates/base.html`
2. Add a route in `app.py` that renders it: `return render_template('<name>.html')`
3. Update `CLAUDE.md` Architecture section with the new route

## Base template conventions

- `templates/base.html` defines the common layout (nav, head, footer)
- Child templates use `{% block content %}{% endblock %}`
- Page title set via `{% block title %}Page Title{% endblock %}`

## Style conventions

- Single stylesheet: `static/css/main.css`
- No external CSS frameworks — keep it minimal and dependency-free
- Mobile-first, use CSS custom properties for colors/spacing

## After any frontend change

- Verify the page renders correctly by running `python app.py` and checking in a browser
- Update `CLAUDE.md` if a new route or template was added
