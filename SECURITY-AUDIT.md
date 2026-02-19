# Security audit notes

This repository is a Hugo theme with vendored front-end assets. During review, the following security issues were identified.

## Fixed in this change

1. **Reverse tabnabbing risk in navbar brand link**
   - `target="_blank"` was used without `noopener`/`noreferrer`.
   - Updated to include `rel="... noopener noreferrer"`.

2. **Protocol-relative third-party script URLs**
   - Highlight.js and MathJax were loaded via `//...` URLs.
   - Updated to explicit `https://` URLs to avoid insecure loading in misconfigured contexts.

3. **Outdated Highlight.js default version**
   - Default Highlight.js version was `9.9.0` (very old and no longer supported).
   - Updated default to `11.11.1` and switched init call to `hljs.highlightAll()` for compatibility.

4. **Deprecated MathJax CDN URL**
   - `cdn.mathjax.org` is retired.
   - Updated to jsDelivr-hosted MathJax v2 URL.

## Remaining risks to address

1. **Vendored legacy JavaScript dependencies are still old**
   - `js/core/jquery.3.2.1.min.js`
   - `js/core/bootstrap.min.js` (Bootstrap 4-era build)
   - `js/plugins/bootstrap-datepicker.js`
   - `js/plugins/bootstrap-switch.js`

   These files should be upgraded to currently supported releases and regression-tested. As vendored static artifacts, this should be done intentionally in a dedicated update PR.

2. **Hard-coded `http://` URLs in social meta partial**
   - `layouts/partials/meta.html` contains several `http://` values.
   - While these are metadata URLs and not executable scripts, converting to `https://` avoids mixed-content/privacy concerns and improves transport security consistency.
