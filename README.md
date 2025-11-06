# DNS Configuration and GitHub Pages Integration

This technical document describes how to configure DNS records in **Cloudflare** to point subdomains to **GitHub Pages**, and explains the differences between two connection methods.


## 1. Redirect `github.amadorh.com` to GitHub Pages

**Goal:**
Create a DNS record in Cloudflare so that the subdomain `github.amadorh.com` points to a GitHub Page under `jhingalv.github.io` that displays the message:

> “Welcome to my GitHub”

### DNS Record Type

**CNAME** (Canonical Name Record)

### DNS Record Type

This is a DNS-level redirection — **no HTML, PHP, or .htaccess redirection** is used.

### Steps in Cloudflare

1. Log in to your **Cloudflare Dashboard** and select your domain.
2. Go to **DNS > Records** and create a new record:

   * **Type:** CNAME
   * **Name:** `github`
   * **Target:** `jhingalv.github.io`
   * **Proxy status:** **DNS only** (gray cloud — GitHub handles HTTPS)

### Steps in GitHub
  
1. In your GitHub repository:

   * Go to **Settings > Pages**.
   * Under **Custom domain**, input:
     ```
     github.amadorh.com
     ```
   * Save and wait for GitHub to automatically create or update the `CNAME` file in the repository.

**Expected Result:**
Visiting `https://github.amadorh.com` points to your GitHub Pages site showing “Welcome to my GitHub”.

## 2. Point `aboutme.amadorh.com` directly to your GitHub profile README

**Goal:**
Create a DNS record that directly points the subdomain `aboutme.amadorh.com` to our profile README repo (the one that is named just like our username) and verify it in GitHub’s domain settings.

### DNS Record Type

**CNAME** (Canonical Name Record)

1. In cloudflare, access to **DNS > Records**, create a new record:

   * **Type:** CNAME
   * **Name:** `aboutme`
   * **Target:** `jhingalv.github.io`
   * **Proxy status:** **DNS only** (gray cloud — GitHub handles HTTPS)
2. In your GitHub repository:

   * Go to **Settings > Pages**.
   * Under **Custom domain**, input:
     ```
     aboutme.amadorh.com
     ```
   * Save and wait for GitHub to automatically create or update the `CNAME` file in the repository.
3. In your GitHub profile:

   * On your profile, navigate to **Settings > Pages > Verified domains**
   * Input our domain `aboutme.amadorh.com` and click on **Add Domain**
   * GitHub will provide us with a DNS record:
     * **Type:** TXT
     * **Name:** _github-pages-challenge-jhingalv.amadorh.com
     * **Content:** 8a888m888ad88orh888as88i88r888 (note that this is not the real code)
4. Go back to Cloudflare, navigate to **DNS > Records** and create a new **TXT record** with the information provided from github.
5. In GitHub, click on verify. 

**Expected Result:**
Visiting `https://aboutme.amadorh.com` loads your **GitHub profile README** site directly under your custom subdomain.
