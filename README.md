# 🛠 Fix WordPress URL After Domain Change (WP-CLI Guide)

If you've changed your WordPress site's domain (e.g., from `http://my-first-skool.local` to `http://localhost:10003`), this guide helps you fix all broken URLs, assets, and paths using WP-CLI.

---

## 📦 Requirements

- WP-CLI installed (https://wp-cli.org/)
- Access to your WordPress root directory
- Correct PHP and database setup
- Terminal/shell access

---

## 🔁 Replace Old URL with New URL

Run this command from the root directory of your WordPress installation:

```bash
wp search-replace 'http://my-first-skool.local' 'http://localhost:10003' --skip-columns=guid --all-tables

```



## Run this commands in sql (Multisite)
```bash
-- Update site URL in wp_site table
UPDATE wp_site SET domain = 'localhost:10003' WHERE id = 1;

-- Update home and siteurl in wp_blogs table
UPDATE wp_blogs SET domain = 'localhost:10003' WHERE blog_id = 1;

-- Update options table for the main site
UPDATE wp_options SET option_value = 'http://localhost:10003' 
WHERE option_name IN ('siteurl', 'home');

-- If you have subsites, update them too
UPDATE wp_options SET option_value = REPLACE(option_value, 'https://lsstg.wpenginepowered.com', 'http://localhost:10003');

-- Update post content and meta
UPDATE wp_posts SET post_content = REPLACE(post_content, 'https://lsstg.wpenginepowered.com', 'http://localhost:10003');
UPDATE wp_postmeta SET meta_value = REPLACE(meta_value, 'https://lsstg.wpenginepowered.com', 'http://localhost:10003');
```


✅ **What it does:**

- Replaces all instances of the old domain in the database  
- Works across all tables and serialized data  
- Skips the `guid` column (optional but recommended when migrating dev URLs)  


💡 **Other Useful WP-CLI Commands**

### 🔍 Test What Will Be Replaced (Dry Run)
```bash
wp search-replace 'old-url' 'new-url' --dry-run
````

*Use this to preview what will change, without making changes.*

---

### 🧹 Clear Transients (cached data)

```bash
wp transient delete --all
```

---

### 🔒 Regenerate `.htaccess` (after permalink changes)

```bash
wp rewrite flush
```

---

### 🔄 Reset Permalink Structure

```bash
wp option update permalink_structure '/%postname%/'
wp rewrite flush
```

---

### 🧠 Useful Info Commands

```bash
wp option get siteurl
wp option get home
wp theme list
wp plugin list
```

---

⚠️ **Notes**

* Always backup your database before running bulk operations
* After replacing URLs, clear your browser cache
* Go to WordPress **Settings > Permalinks** and click **Save Changes** to refresh rewrite rules

```

Just copy and paste that into a file named `README.md` and you’re good to go!
```
