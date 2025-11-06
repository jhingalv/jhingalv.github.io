# Adding a DNS record to a sub-domain zone

This technical document describes how to configure DNS records in **Cloudflare** to point subdomains to **GitHub Pages**, and explains the differences between two connection methods.

## 1. Redirect `github.amadorh.com` to GitHub Pages

**Goal:**
Create a DNS record in Cloudflare so that the subdomain `github.amadorh.com` points to a GitHub Page under `jhingalv.github.io` that displays the message:

> “Welcome to my GitHub”

### DNS Record Type

**CNAME** (Canonical Name Record)

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
Create a DNS record that directly points the subdomain `aboutme.amadorh.com` to our profile README repository (the one that is named just like our username) and verify it in GitHub’s domain settings.

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

## 3. Differences Between the Two DNS Configurations

Both configurations use a **CNAME** record to redirect subdomains to **GitHub Pages**, but there are key differences in how DNS records are managed and the purpose of each redirection.

### **CNAME and DNS Records:**
- **For `github.amadorh.com`:**  
  Only a **CNAME** pointing to `jhingalv.github.io` is required. GitHub handles everything else, such as HTTPS management and subdomain resolution. No additional records are necessary for domain verification.

- **For `aboutme.amadorh.com`:**  
  Similar to the previous case, a **CNAME** is created pointing to `jhingalv.github.io`, but GitHub also requires a **TXT** record to verify the subdomain ownership. This step is necessary to associate the subdomain with the GitHub profile, and the **TXT** record must be added in **Cloudflare**.

### **TXT Records and Domain Verification:**
- **For `github.amadorh.com`:**  
  No **TXT** record is required, as it only redirects to a repository and GitHub does not need to verify domain ownership.

- **For `aboutme.amadorh.com`:**  
  GitHub requires a **TXT** record to verify domain control. This is necessary to associate the subdomain with the user's profile and display the content of the `README.md` file from their GitHub account.

### **Purpose and Security Requirements:**
- **For `github.amadorh.com`:**  
  The **CNAME** is sufficient for redirection, with no need for additional domain verification, making it easier to configure.
  
- **For `aboutme.amadorh.com`:**  
  The **TXT** verification adds an extra layer of security. GitHub needs to ensure that you control the subdomain before allowing it to be linked to your profile.
  
