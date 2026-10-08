# Public site (jdkno.com/fin)

Static pages for the Fin app. This folder is **not** part of the app, and it has no secrets: publish it as its own
public repository (GitHub Pages serves a repo's root or its /docs folder, and the app's `docs/` folder is private).

- `fin/privacy/index.html`: the privacy policy (English and Spanish on one page). The App Store Connect
  "Privacy Policy URL" is `https://jdkno.com/fin/privacy`.

## Serving it at jdkno.com/fin/privacy

1. Create a public repo (for example `jdkno-site`), copy this folder's contents to its root, and turn on GitHub Pages
   (Settings › Pages › Deploy from branch › main / root). It is then at
   `https://<user>.github.io/jdkno-site/fin/privacy/`.
2. In Cloudflare, jdkno.com needs a proxied (orange cloud) DNS record at its root. If it has no website, add an
   `A` record `@` → `192.0.2.1`, proxied: nothing is ever fetched from it.
3. Add a Worker with the route `jdkno.com/fin/*`:

```js
export default {
  async fetch(request) {
    const url = new URL(request.url);
    const target = "https://<user>.github.io/jdkno-site" + url.pathname + url.search;
    return fetch(target, { headers: { "User-Agent": "jdkno-site-proxy" } });
  },
};
```

Check `https://jdkno.com/fin/privacy` shows the page (the trailing slash is added by GitHub Pages).
