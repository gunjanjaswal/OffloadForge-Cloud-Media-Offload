# OffloadForge

<p align="center">
  <img src="https://img.shields.io/badge/WordPress-5.3%2B-21759B?logo=wordpress&logoColor=white" alt="WordPress 5.3+">
  <img src="https://img.shields.io/badge/PHP-7.4%2B-777BB4?logo=php&logoColor=white" alt="PHP 7.4+">
  <img src="https://img.shields.io/badge/Tested%20up%20to-7.1-21759B?logo=wordpress&logoColor=white" alt="Tested up to 7.1">
  <img src="https://img.shields.io/badge/License-GPLv2%2B-3DA639?logo=gnu&logoColor=white" alt="License GPLv2+">
  <img src="https://img.shields.io/badge/version-1.3.8-brightgreen" alt="Version 1.3.8">
  <img src="https://img.shields.io/badge/build-no%20build%20step-blue" alt="No build step">
</p>

<p align="center">
  <b>Offload your WordPress media to the cloud and serve it over a CDN.</b><br>
  Amazon S3, DigitalOcean Spaces, or Google Cloud Storage, with no SDK and no Composer step.
</p>

---

## Why it exists

Every image, video, and PDF in your media library is served from your own web server by default. On a busy site that's bandwidth you pay for and latency your visitors feel, especially the ones far from your host.

OffloadForge copies your media to a cloud bucket **you** own and rewrites the URLs so files load from the cloud or your CDN instead. New uploads sync on their own, and a bulk tool moves everything you already have. It talks to each provider's REST API directly through WordPress's built-in HTTP functions with AWS Signature V4 signing, so there's nothing to compile and no library to keep updated.

Your credentials and media only ever go to the bucket and region you configure. Nothing is sent to the plugin author or any third party.

## Features

| | |
|---|---|
| **Multi-cloud** | Amazon S3, DigitalOcean Spaces, and Google Cloud Storage (via HMAC keys). |
| **Automatic sync** | New uploads, all image sizes, videos, and documents are copied to the cloud as they're added. |
| **Bulk migration** | One click moves your existing library, with progress tracking, batching, and auto-retry on connection timeouts (handles Cloudflare 524s). |
| **Full URL rewriting** | Post content, `srcset`, thumbnails, theme templates, headers, and footers. An output-buffer pass catches even hardcoded theme URLs. |
| **CDN ready** | Point it at CloudFront or any custom CDN domain for global delivery. |
| **Smart re-link** | Detects files already in the cloud and skips re-uploading them. |
| **Repair tools** | Fix Permissions, Fix Thumbnails, and Fix URLs, each with a Scan → Review → Fix flow. |
| **Safe to leave** | Restore local files anytime, plus a warning before deactivating if your only copies live in the cloud. |
| **Zero dependencies** | No Composer, no SDKs. Works on any WordPress host out of the box. |

## Admin pages

- **Settings** — provider, credentials, CDN domain, path prefix, and whether to remove local files after offload.
- **Bulk Offload** — migrate the whole library with live progress and auto-retry.
- **Restore Local** — pull cloud files back to the server before deactivating.
- **Fix Permissions** — scan for 403 AccessDenied errors and set public-read ACL (or re-upload).
- **Fix Thumbnails** — find offloaded images missing thumbnail sizes in the cloud and upload them.
- **Fix URLs** — update stored URLs after a CDN, bucket, or region change, without re-uploading.

## Installation

**From your dashboard**

1. Download this repository as a ZIP.
2. Go to **Plugins → Add New → Upload Plugin** and pick the ZIP.
3. Activate it.

**Manually**

1. Copy the `g33ki-cloud-storage-for-media-library` folder into `wp-content/plugins/`.
2. Activate **OffloadForge** on the Plugins screen.

## How to use it

1. **Configure your provider.** Open **OffloadForge → Settings** and enter your access key, secret key, bucket, and region. Add a CDN domain if you serve through one.
2. **Test the connection.** The built-in tester checks your credentials before anything goes live.
3. **Let new uploads sync.** From here on, anything you add to the media library is copied to the cloud automatically.
4. **Move what you already have.** Run **Bulk Offload** to migrate your existing library. Progress and errors are shown as it goes, and it retries on timeouts.
5. **Serve from the cloud.** Media URLs are rewritten to your cloud or CDN URLs across the front end.
6. **Repair if needed.** If a bucket policy, CDN domain, or region changes, the Fix Permissions, Fix Thumbnails, and Fix URLs tools put things right without a full re-upload.

## Screenshot

![OffloadForge settings and bulk offload](.wordpress-org/screenshot-1.png)

## Supported providers

**Amazon S3** — object storage with global infrastructure and CloudFront CDN integration.

**DigitalOcean Spaces** — S3-compatible storage with simple, predictable pricing and a built-in CDN.

**Google Cloud Storage** — Google's storage with multi-regional redundancy and edge caching, authenticated with HMAC keys.

## Frequently asked questions

**Do my credentials or media go anywhere except my own bucket?**
No. The plugin connects only to the provider, bucket, and region you set on the Settings screen, using your own account credentials. Nothing is sent to the plugin author or any other party.

**Do I need Composer or an SDK?**
No. It uses WordPress's built-in HTTP API with AWS Signature V4 signing, so there's no build step and nothing extra to install.

**What happens to files I already uploaded before installing?**
Run Bulk Offload. It migrates your existing library in batches, tracks progress, and retries on connection timeouts.

**Can I undo it?**
Yes. Restore Local downloads your cloud media back to the server, and the plugin warns you before deactivating if local copies are missing.

**Will this make my site faster or rank better?**
It moves media traffic off your web server and can serve files closer to visitors, but real results depend on your host, provider, CDN, and theme, so no specific speed or SEO outcome is promised.

## Requirements

- WordPress 5.3 or newer
- PHP 7.4 or newer

## Support

If OffloadForge saves you some bandwidth, you can [buy me a coffee on Ko-fi](https://ko-fi.com/gunjanjaswal). It genuinely helps.

Found a bug or have an idea? Open an issue, or email me at [hello@gunjanjaswal.me](mailto:hello@gunjanjaswal.me).

## Author

**Gunjan Jaswal**

- Website: [gunjanjaswal.me](https://www.gunjanjaswal.me)
- Email: [hello@gunjanjaswal.me](mailto:hello@gunjanjaswal.me)
- Ko-fi: [ko-fi.com/gunjanjaswal](https://ko-fi.com/gunjanjaswal)

## Credits

Thanks to [pumamood](https://github.com/pumamood) for the admin-screen XSS hardening in 1.3.8 ([#1](https://github.com/gunjanjaswal/OffloadForge-Cloud-Media-Offload/pull/1)).

## License

Released under the [GPLv2 or later](https://www.gnu.org/licenses/gpl-2.0.html).

---

<sub>Not affiliated with, endorsed by, or sponsored by Amazon, DigitalOcean, or Google. "Amazon S3", "DigitalOcean Spaces", and "Google Cloud Storage" are trademarks of their respective owners.</sub>
