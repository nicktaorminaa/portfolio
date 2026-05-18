/* =========================================================
   Nicholas Taormina — Portfolio
   Theme, nav, scroll reveal, scroll-spy, contact form
   ========================================================= */

(function () {
    'use strict';

    /* ----- Theme toggle (persists in localStorage; respects OS) ----- */
    const root = document.documentElement;
    const themeKey = 'nt-portfolio-theme';
    const stored = (() => {
        try { return localStorage.getItem(themeKey); } catch (e) { return null; }
    })();
    const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
    const initialTheme = stored || (prefersDark ? 'dark' : 'light');
    root.setAttribute('data-theme', initialTheme);

    const themeToggle = document.getElementById('themeToggle');
    if (themeToggle) {
        themeToggle.addEventListener('click', () => {
            const current = root.getAttribute('data-theme') === 'dark' ? 'dark' : 'light';
            const next = current === 'dark' ? 'light' : 'dark';
            root.setAttribute('data-theme', next);
            try { localStorage.setItem(themeKey, next); } catch (e) { /* ignore */ }
        });
    }

    /* ----- Mobile nav toggle ----- */
    const navToggle = document.getElementById('navToggle');
    const navLinks = document.getElementById('navLinks');
    if (navToggle && navLinks) {
        navToggle.addEventListener('click', () => {
            const open = navLinks.classList.toggle('open');
            navToggle.setAttribute('aria-expanded', String(open));
        });
        navLinks.querySelectorAll('a').forEach(a => {
            a.addEventListener('click', () => {
                navLinks.classList.remove('open');
                navToggle.setAttribute('aria-expanded', 'false');
            });
        });
    }

    /* ----- Nav border on scroll ----- */
    const nav = document.getElementById('nav');
    const onScroll = () => {
        if (!nav) return;
        if (window.scrollY > 8) nav.classList.add('scrolled');
        else nav.classList.remove('scrolled');
    };
    onScroll();
    window.addEventListener('scroll', onScroll, { passive: true });

    /* ----- Reveal on scroll (IntersectionObserver) ----- */
    const reveals = document.querySelectorAll('.reveal');
    if ('IntersectionObserver' in window && reveals.length) {
        const io = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('in');
                    io.unobserve(entry.target);
                }
            });
        }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
        reveals.forEach(el => io.observe(el));
    } else {
        reveals.forEach(el => el.classList.add('in'));
    }

    /* ----- Active section highlight in nav ----- */
    const sections = document.querySelectorAll('main section[id]');
    const linkMap = new Map();
    document.querySelectorAll('.nav__links a').forEach(a => {
        const id = a.getAttribute('href');
        if (id && id.startsWith('#')) linkMap.set(id.slice(1), a);
    });
    if ('IntersectionObserver' in window && sections.length) {
        const spy = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                const id = entry.target.id;
                const link = linkMap.get(id);
                if (!link) return;
                if (entry.isIntersecting) {
                    linkMap.forEach(a => a.classList.remove('active'));
                    link.classList.add('active');
                }
            });
        }, { rootMargin: '-45% 0px -50% 0px', threshold: 0 });
        sections.forEach(s => spy.observe(s));
    }

    /* ----- Year in footer ----- */
    const yearEl = document.getElementById('year');
    if (yearEl) yearEl.textContent = new Date().getFullYear();

    /* ----- Contact form (mailto fallback — no backend) ----- */
    const form = document.getElementById('contactForm');
    const note = document.getElementById('formNote');
    if (form) {
        form.addEventListener('submit', (e) => {
            e.preventDefault();
            const name = form.name.value.trim();
            const email = form.email.value.trim();
            const message = form.message.value.trim();

            if (!name || !email || !message) {
                if (note) { note.textContent = 'Please fill out every field before sending.'; note.className = 'form-note error'; }
                return;
            }
            const emailOk = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
            if (!emailOk) {
                if (note) { note.textContent = 'That email address looks off — mind double-checking?'; note.className = 'form-note error'; }
                return;
            }

            const subject = encodeURIComponent(`Portfolio inquiry from ${name}`);
            const body = encodeURIComponent(`${message}\n\n— ${name}\n${email}`);
            window.location.href = `mailto:nicholas.taormina11@gmail.com?subject=${subject}&body=${body}`;

            if (note) { note.textContent = 'Opening your email client…'; note.className = 'form-note success'; }
            form.reset();
        });
    }
})();
