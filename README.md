# OffloadForge

<p align="center">
  <img src="https://img.shields.io/badge/WordPress-5.3%2B-21759B?logo=wordpress&logoColor=white" alt="WordPress 5.3+">
  <img src="https://img.shields.io/badge/PHP-7.4%2B-777BB4?logo=php&logoColor=white" alt="PHP 7.4+">
  <img src="https://img.shields.io/badge/Tested%20up%20to-7.1.4-21759B?logo=wordpress&logoColor=white" alt="Tested up to 7.1.4">
  <img src="https://img.shields.io/badge/License-GPLv2%2B-3DA639?logo=gnu&logoColor=white" alt="License GPLv2+">
  <img src="https://img.shields.io/badge/version-1.4.1-brightgreen" alt="Version 1.4.1">
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

## Companion plugin: Unique Upload Filenames

If you turn on **"remove local files"**, OffloadForge deletes the local copy of each file after it reaches your bucket. That saves disk space, but it also takes away the only thing WordPress uses to keep filenames unique: its duplicate check only looks at the local uploads folder, which is now empty. So a second `photo.jpg` (or the generic `unknown.png` that a lot of AI image tools export) keeps its name and quietly overwrites the first one on your bucket. The symptom is confusing: one post suddenly shows another post's image.

[**Unique Upload Filenames**](https://github.com/gunjanjaswal/Wordpress-Unique-Upload) is a tiny, free companion plugin that closes this gap. It gives every new upload a short unique suffix before the file is written, so two uploads can never land on the same path. It is zero-config and leaves existing files untouched. Recommended for any OffloadForge site running with "remove local files" enabled.

## Changelog

### 1.4.1
- Tested and confirmed compatible with WordPress 7.1.4.

### 1.4.0
- New: a Status screen that shows how much of your media library is offloaded at a glance, with a progress bar, counts of offloaded versus local-only files, your active provider, and a shortcut to offload whatever is left.

### 1.3.9
- Added a tip under the "Remove Local Files" setting, and a matching FAQ and support note, recommending the free companion plugin Unique Upload Filenames. When the local copy is deleted after offload, WordPress can no longer catch duplicate filenames, so two same-name uploads can overwrite each other on the bucket; the companion plugin gives every upload a unique name to prevent that.
- Rounded out the support links (WordPress.org forum, GitHub issues, email).

### 1.3.8
- Hardened the admin screens against cross-site scripting. The messages and file details that come back from the server, test and save notices, the bulk offload and restore error lists, and the broken-file and thumbnail scan tables, are now escaped before they are shown. A media item whose title or error text happens to contain HTML can no longer have that HTML run as markup in your dashboard.
- Nothing changes in how any of those screens look or work for normal file names; only untrusted markup is neutralised.
- Thanks to pumamood for reporting this and sending the first fix.

### 1.3.7
- Fixed thumbnails going missing on WordPress 7.1, where the browser can now do the image processing itself. In that flow WordPress runs the attachment metadata filter twice, once before any thumbnail exists and again once they have all been sent up, and OffloadForge was doing its work on the first pass. It now waits for the pass that actually carries the sizes, so every thumbnail reaches your bucket.
- Same fix closes a worse case: with "remove local files" switched on, the old behaviour deleted the original while the browser was still uploading thumbnails for it.
- Nothing changes for uploads that WordPress processes on the server, including anything added by WP-CLI or another plugin.

### 1.3.6
- Added a safety warning for when your media lives only in the cloud. If you turned on "remove local files" and offloaded, deactivating the plugin would break those URLs, so the plugin now watches for that state and warns you before it bites.
- The warning shows on every admin screen (including the plugin's own pages) and has a close button, so you can dismiss it once you've read it.
- It also appears as a highlighted note pinned right under OffloadForge on the Plugins page, where deactivation actually happens, so the risk is hard to miss. Both spots link straight to Restore Local so you can pull files back first.

### 1.3.5
- Fixed a PHP "Array to string conversion" warning triggered when an image was requested by explicit width/height dimensions (an array size) instead of a named size. The attachment-image-src filter now guards the per-size meta lookup, so array sizes cleanly fall back to the full-size cloud URL. No change for named sizes.
- Removed the lightning emoji from the admin menu label so the sidebar shows a single cloud icon (the emoji rendered like a duplicate icon next to the dashicon).

### 1.3.4
- Updated the plugin's GitHub links (Plugin URI and the bug-report link) to the current repository. No functional change.

### 1.3.3
- Renamed the plugin to "OffloadForge - Cloud Media Offload for S3, DigitalOcean Spaces & Google Cloud". The title now leads with the distinctive OffloadForge brand. The slug (`g33ki-cloud-storage-for-media-library`), the `g33ki_settings` option, and all stored data are unchanged, so this updates in place with no reconfiguration. Admin menu, settings heading, and docs follow the new name.

### 1.3.2
- Renamed the plugin to "G33ki Cloud Media Offload for S3, DigitalOcean Spaces & Google Cloud" so the title leads with the distinctive g33ki brand and places the provider names after "for". The slug (`g33ki-cloud-storage-for-media-library`), the `g33ki_settings` option, and all stored data are unchanged, so this updates in place with no reconfiguration.
- Added a clear statement that the plugin is an independent integration and is not affiliated with Amazon, DigitalOcean, or Google.
- Rewrote the description to drop comparative and promotional wording and to remove SEO/ranking claims the plugin cannot guarantee.
- Expanded the External Services section to state that the plugin connects only to the provider, bucket, and region you configure with your own credentials, and sends nothing to the author.
- Security hardening: provider classes are now resolved through an explicit allowlist (`G33KI_Provider_Base::class_for()`) instead of being built from the stored provider value, so a class name can never be constructed from request-supplied data.
- Fixed the internal `G33KI_VERSION` constant, which still read 1.3.0.

### 1.3.1
- Fix: corrected the declared minimum WordPress version. The plugin calls `wp_create_image_subsizes()` when regenerating a missing thumbnail, which needs WordPress 5.3, while the headers claimed support back to 5.0. The call was already wrapped in a `function_exists()` guard so nothing ever fatalled — on 5.0-5.2 the regeneration simply, and silently, did nothing. The headers now state 5.3 so the requirement is honest and the official Plugin Check passes cleanly.

### 1.3.0
- Renamed the plugin display title to "Cloud Storage For Media Library — S3, DigitalOcean Spaces & Google Cloud" for clarity in search and listings. The plugin slug, settings (`g33ki_settings`), and all stored data are unchanged — this is a display-only rebrand and updates safely with no reconfiguration required.
- Cleaned up provider labels on the admin screen (removed the redundant brand suffix).

### 1.2.4
- WordPress 7.0 Connectors API integration: registers Amazon S3, DigitalOcean Spaces, and Google Cloud Storage as `cloud_storage` connectors on the `wp_connectors_init` action. Each connector links to this plugin's settings page for credential management.
- The plugin's settings array (`g33ki_settings`) stores multiple credentials per provider (access_key, secret_key, bucket, region). The Connectors API's `api_key` method handles a single setting value, so connectors are registered with `method: none` and the credentials_url points back to this plugin's settings screen. Full multi-field central management will land once core supports it (or after a future option refactor).
- Added `g33ki_register_connectors` action so third-party code can register richer connectors against the same providers.

### 1.2.3
- Updated "Tested up to" to WordPress 7.0.
- Bumped minimum PHP requirement to 7.4 (WordPress 7.0 dropped support for PHP 7.2 and 7.3).
- Added "Support on Ko-fi" (https://ko-fi.com/gunjanjaswal) and "Contact Developer" links to plugin row meta on the Plugins screen.
- Added `Requires at least`, `Tested up to`, and `Requires PHP` headers to the main plugin file.

### 1.2.2
- Rebranding: Renamed to G33ki Cloud Storage For Media Library
- SEO: Improved description for better search visibility on "offload media library" and "move media to cloud" keywords
- Cleanup: Removed Buy Me a Coffee links

### 1.2.1
- Fix: Removed inline script tag from fix-urls.php template to adhere to WP enqueuing guidelines
- Fix: Replaced ob_start() full-page buffering with wp_template_enhancement_output_buffer
- Docs: Added External Services declaration block to document S3/DO/GCS usage

### 1.2.0
- New: Full-page output buffer URL rewriting — catches theme-hardcoded image URLs in headers, footers, and templates
- New: Handles http/https URL variations and relative /wp-content/uploads/ paths
- New: Filters for post_thumbnail_html, widget_text, custom_logo, wp_get_attachment_image, header_image_tag
- Fix: Images in theme templates not rewriting for non-logged-in users

### 1.1.0
- New: Fix Permissions tool — scan and repair 403 AccessDenied cloud files
- New: Fix Thumbnails tool — find and upload missing thumbnail sizes to cloud
- New: Fix URLs tool — bulk-update stale URLs after CDN/bucket/region changes
- New: Auto-retry on connection timeouts (up to 5 retries per batch)
- New: Smart re-link — detects existing cloud files and skips re-upload
- New: Set public-read ACL when re-linking existing cloud files
- Fix: Bulk offload offset logic causing premature "completed" status
- Fix: Batch size reduced to avoid Cloudflare 524 timeout errors
- Fix: Success message no longer persists after page reload

### 1.0.0
- Initial release
- Support for Amazon S3, DigitalOcean Spaces, and Google Cloud Storage
- No external SDK dependencies — uses built-in HTTP API with request signing
- Automatic upload synchronization for new media
- Bulk offload tool for existing media with batch processing
- CDN integration support (CloudFront, custom domains)
- Built-in connection testing
- Optional local file removal after cloud upload
- Restore local files tool — download cloud media back to server
- Deactivation safety warning when local files are missing
- Custom path prefix support

## Support

Need a hand? Here is where to reach me:

- **WordPress.org support forum:** [the plugin's support page](https://wordpress.org/support/plugin/g33ki-cloud-storage-for-media-library/)
- **Bugs and ideas:** open an [issue on GitHub](https://github.com/gunjanjaswal/OffloadForge-Cloud-Media-Offload/issues)
- **Email:** [hello@gunjanjaswal.me](mailto:hello@gunjanjaswal.me)

If OffloadForge saves you some bandwidth, you can [buy me a coffee on Ko-fi](https://ko-fi.com/gunjanjaswal). It genuinely helps.

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
