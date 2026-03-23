# Form Design for Awwwards-Level Sites
## Contact forms, newsletter signups, validation, and premium input styling

Forms are where Usability (30% of score) wins or dies. A generic form kills the vibe; a designed one shows craft.

---

## CONTACT FORM (minimal, editorial)

```html
<form class="contact-form" id="contactForm" novalidate>
  <div class="form-group">
    <label for="name" class="form-label">Name</label>
    <input type="text" id="name" name="name" class="form-input" required autocomplete="name" />
    <div class="form-line"></div>
  </div>

  <div class="form-group">
    <label for="email" class="form-label">Email</label>
    <input type="email" id="email" name="email" class="form-input" required autocomplete="email" />
    <div class="form-line"></div>
  </div>

  <div class="form-group">
    <label for="message" class="form-label">Message</label>
    <textarea id="message" name="message" class="form-input form-textarea" rows="4" required></textarea>
    <div class="form-line"></div>
  </div>

  <button type="submit" class="form-submit" data-magnetic>
    <span>Send Message</span>
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M7 17L17 7M17 7H7M17 7V17"/>
    </svg>
  </button>

  <div class="form-status" id="formStatus" aria-live="polite"></div>
</form>
```

---

## PREMIUM INPUT STYLING

```css
.form-group {
  position: relative;
  margin-bottom: 2.5rem;
}

.form-label {
  display: block;
  font-size: 0.65rem;
  font-weight: 500;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 0.5rem;
}

.form-input {
  width: 100%;
  padding: 0.8rem 0;
  font-size: 1.1rem;
  font-family: inherit;
  color: var(--ink);
  background: transparent;
  border: none;
  border-bottom: 1px solid var(--border);
  outline: none;
  transition: border-color 0.3s ease;
  /* Prevent iOS zoom */
  font-size: 16px;
}

.form-input:focus {
  border-bottom-color: var(--accent);
}

/* Animated underline */
.form-line {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--accent);
  transition: width 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.form-input:focus ~ .form-line {
  width: 100%;
}

/* Textarea */
.form-textarea {
  resize: vertical;
  min-height: 100px;
}

/* Submit button */
.form-submit {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  padding: 1rem 2.5rem;
  border: 1px solid var(--ink);
  background: transparent;
  color: var(--ink);
  font-family: inherit;
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  cursor: pointer;
  transition: background 0.4s ease, color 0.4s ease;
}
.form-submit:hover {
  background: var(--ink);
  color: var(--bg);
}
.form-submit svg {
  transition: transform 0.3s ease;
}
.form-submit:hover svg {
  transform: translate(3px, -3px);
}

/* Placeholder styling */
.form-input::placeholder {
  color: var(--ink-faint);
  font-style: italic;
}
```

---

## VALIDATION

```javascript
function initFormValidation() {
  const form = document.getElementById('contactForm');
  const status = document.getElementById('formStatus');

  form?.addEventListener('submit', (e) => {
    e.preventDefault();
    const errors = [];

    // Validate
    const name = form.querySelector('#name');
    const email = form.querySelector('#email');
    const message = form.querySelector('#message');

    if (!name.value.trim()) errors.push({ el: name, msg: 'Name is required' });
    if (!email.value.trim() || !email.value.includes('@')) {
      errors.push({ el: email, msg: 'Valid email is required' });
    }
    if (!message.value.trim()) errors.push({ el: message, msg: 'Message is required' });

    // Show errors
    form.querySelectorAll('.form-error').forEach(e => e.remove());
    form.querySelectorAll('.form-input').forEach(i => i.classList.remove('error'));

    if (errors.length > 0) {
      errors.forEach(({ el, msg }) => {
        el.classList.add('error');
        const errEl = document.createElement('span');
        errEl.className = 'form-error';
        errEl.textContent = msg;
        el.parentNode.appendChild(errEl);
      });
      // Shake animation
      gsap.fromTo(form, { x: -8 }, { x: 0, duration: 0.5, ease: 'elastic.out(1, 0.3)' });
      return;
    }

    // Success
    status.textContent = 'Message sent successfully!';
    status.className = 'form-status success';
    gsap.from(status, { opacity: 0, y: 10, duration: 0.5 });
    form.reset();
  });
}
```

```css
.form-input.error { border-bottom-color: #D32F2F; }
.form-input.error ~ .form-line { background: #D32F2F; width: 100%; }

.form-error {
  display: block;
  font-size: 0.7rem;
  color: #D32F2F;
  margin-top: 0.3rem;
  letter-spacing: 0.05em;
}

.form-status {
  margin-top: 1.5rem;
  font-size: 0.85rem;
  padding: 0.8rem 1.2rem;
}
.form-status.success {
  color: #2E7D32;
  border-left: 3px solid #2E7D32;
}
```

---

## NEWSLETTER SIGNUP (inline)

```html
<div class="newsletter">
  <p class="newsletter-label">Stay in the loop</p>
  <form class="newsletter-form">
    <input type="email" placeholder="your@email.com" class="newsletter-input" required />
    <button type="submit" class="newsletter-btn" aria-label="Subscribe">→</button>
  </form>
</div>
```

```css
.newsletter-form {
  display: flex;
  border-bottom: 1px solid var(--ink);
  max-width: 400px;
}

.newsletter-input {
  flex: 1;
  padding: 0.8rem 0;
  font-size: 1rem;
  font-family: inherit;
  background: none;
  border: none;
  color: var(--ink);
  outline: none;
  font-size: 16px; /* prevent iOS zoom */
}

.newsletter-btn {
  background: none;
  border: none;
  color: var(--ink);
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.5rem;
  min-width: 44px;
  min-height: 44px;
  transition: transform 0.3s ease;
}
.newsletter-btn:hover { transform: translateX(4px); }

.newsletter-label {
  font-size: 0.65rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 0.5rem;
}
```

---

## RULES

1. **All inputs ≥16px font size** — prevents iOS auto-zoom
2. **Labels above inputs** — never float labels inside (accessibility)
3. **`autocomplete` attributes** — set on every input for browser autofill
4. **Error states** must be visible without color alone (use icons or text, not just red)
5. **`novalidate`** on form — use JS validation for styled error messages
6. **`aria-live="polite"`** on status messages — screen readers announce changes
7. **Shake animation** on validation failure — gives tactile error feedback
8. **Magnetic submit button** — use `data-magnetic` for premium feel
