**English** | [简体中文](README.md)

# play-console-signin

A Claude Code skill: fill out Google Play Console's Sign in details
(formerly App access) declaration. Not part of Store listing, yet an
"Incomplete" status here disables the "Submit changes for review" button
on Publishing overview. Generic skill, not tied to any specific project
directory. Body text is Chinese-first in the shipped SKILL.md content
(per this author's default); this top-level README is English for
discoverability. Code, CLI commands, file paths, and technical terms are
kept verbatim either way.

## Install

```
/plugin marketplace add mosjin/play-console-signin-skill
/plugin install signinpub@play-console-signin
```

Invoke with the short name `signinpub` (e.g. `/signinpub:version`,
`/signinpub:upgrade`), following the same convention as
`mosjin/android-voice-test-kit`: a descriptive marketplace/repo name
paired with a short plugin command name.

## What this covers

- The core Sign in details gotcha: after selecting "Yes", a checkbox
  ("Sign in details in this declaration provide full access to all the
  features and content within this app, including premium or paid
  content") is *required*, not optional-looking as it appears — leaving
  it unchecked makes "Add" fail with a generic "Your changes couldn't be
  saved" error that's easy to miss as just more explanatory text.
- The actual test for that checkbox: it asks whether this account can
  reach *all* content, not whether this specific account has paid for
  anything — an app with no real paywall (payment only raises a usage
  quota) should check "Yes".
- How to create a usable review test account: call the project's own
  backend registration API directly to create a dedicated account on
  production, and seed a couple of demo records with the same token —
  don't expect the reviewer to create content themselves.
- Shared with `assetpub`/`datapub`: the form is "not saved until you
  click Save" — reopening the page URL silently discards unsaved input.

## Related

- [`play-console-data-safety-skill`](https://github.com/mosjin/play-console-data-safety-skill)
  (datapub) — the other half of the same Publishing overview
  pre-submission declarations, the Data safety questionnaire.
- [`play-console-assets-skill`](https://github.com/mosjin/play-console-assets-skill)
  (assetpub) — another stage of the same Play Console publishing flow,
  covering store listing graphics upload.
- [`play-console-app-publish`](https://github.com/mosjin/play-console-app-publish-skill)
  (playpub) — the Chrome CDP connection method, app creation, Internal
  testing uploads, subscription product creation.
