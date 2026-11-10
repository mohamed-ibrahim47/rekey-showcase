# Rekey

**Every password, none of them stored.** A strong, different password for every site and app, recomputed when you need it from a secret locked in your phone's secure hardware and opened by your fingerprint.

**[See the full page →](https://mohamed-ibrahim47.github.io/rekey-showcase/)** · **[Privacy policy](https://mohamed-ibrahim47.github.io/rekey-showcase/privacy.html)**

> The source code is private. This page shows what the app does and how it works.

## The idea

A password vault keeps every password, encrypted behind a master password. A master-password generator keeps nothing, but everything falls if someone guesses that one password.

Rekey keeps nothing secret either, and has no password to guess:

- The master is **128 random bits**, created on the phone and imported into the **Android Keystore** as a non-exportable key that needs a fingerprint for every use.
- It's shown once as **12 words** for a paper backup, then wiped from the app.
- Each password is `HMAC-SHA256(master, "rekey-v1" + site + login + version)` inside the secure hardware, stretched in software and mapped onto the allowed characters without bias.

The phone stores only a list of sites, logins, versions and rules. None of it is secret.

## What it does

- **Autofill that knows the site.** The page's domain is reduced to its registrable domain with a bundled public suffix list. Only a matching entry is offered, so a look-alike domain gets nothing.
- **Trusted browsers only.** Any app can claim its fields belong to a website, so a web domain counts only from a browser you confirmed once. Its signing certificate is pinned.
- **https only, one origin per fill.** Native apps are linked once and pinned to their signing certificate.
- **Password changes** by raising a site's version; the previous one stays one touch away.
- **Per-site rules**, **master generations** (move sites to a new master one at a time), **export/import**, and **restore from 12 words**.
- **No internet permission**, screenshots blocked, passwords wiped from memory when you leave the screen, clipboard cleared after 30 s.

## How it's built

- Kotlin + Jetpack Compose, Android 9 and newer.
- A pure-Kotlin `core` module holds the maths, tested against 56 frozen vectors and a reference implementation. The algorithm is frozen as `rekey-v1`.
- Android Keystore (StrongBox when available), with an AES-wrapped fallback for phones that can't import an HMAC key.

---

Made by [Mohamed Ibrahim](https://github.com/mohamed-ibrahim47)
