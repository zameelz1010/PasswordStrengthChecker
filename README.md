# Password Strength Checker

A small Django mini project with a modern, responsive UI (dark/light aware). It analyses a password as you type and shows a strength meter, a requirements checklist, an estimated crack time, entropy, and suggestions. It also has a one-click strong password generator.

No Docker, no database tables, no accounts. Passwords are analysed in memory and never stored or logged.

## Run it

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python manage.py runserver
```

Open http://127.0.0.1:8000/

## Run the tests

```bash
python manage.py test
```

## How it works

- `checker/scoring.py`: the scoring logic (pure Python). Entropy is estimated as `effective_length x log2(character pool)`, with penalties for repeated characters and sequences like `abc`, `123`, `qwerty`. Common passwords (including leet variants like `P@ssw0rd`) are scored as very weak.
- `checker/views.py`: `index` renders the page; `check_password` is a CSRF-protected JSON endpoint at `POST /api/check/`.
- `checker/static/checker/`: `style.css` and `app.js` (debounced live checking, show/hide, generator using `crypto.getRandomValues`).

## Ideas to extend

- Check against a larger breached-password list (for example the Have I Been Pwned k-anonymity API).
- Add a rate limit on `/api/check/`.
- Add a passphrase (word list) generator.

## Notes

Crack-time estimates assume a fast offline attack (10 billion guesses/second) and are only a rough guide. For deployment, set `DJANGO_SECRET_KEY`, `DJANGO_DEBUG=0` and `DJANGO_ALLOWED_HOSTS`.
