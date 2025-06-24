# Complete Guide: Building an Early-Internet Style Technical Blog

## Table of Contents
1. [Introduction](#introduction)
2. [Building the Website](#building-the-website)
3. [Hosting with GitHub Pages](#hosting-with-github-pages)
4. [Custom Domain Setup](#custom-domain-setup)
5. [Adding Retro Animations](#adding-retro-animations)
6. [Deployment Workflow](#deployment-workflow)
7. [Maintenance & Best Practices](#maintenance--best-practices)

## Introduction

This guide covers building a personal technical blog with:
- Early internet aesthetic (monospace fonts, simple borders)
- Tag-based post organization
- Search functionality
- Light/dark mode
- Free hosting with custom domain
- Retro animations that enhance without overwhelming

## Building the Website

### Core Structure

Create an `index.html` file with this complete template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Name - Software Engineer</title>
    <style>
        /* CSS Variables for Theming */
        :root {
            --bg-color: #ffffff;
            --text-color: #000000;
            --link-color: #0000ff;
            --visited-color: #551a8b;
            --border-color: #cccccc;
            --code-bg: #f0f0f0;
            --tag-bg: #e0e0e0;
            --highlight: #ffff00;
        }

        [data-theme="dark"] {
            --bg-color: #0a0a0a;
            --text-color: #00ff00;
            --link-color: #00ffff;
            --visited-color: #ff00ff;
            --border-color: #333333;
            --code-bg: #1a1a1a;
            --tag-bg: #2a2a2a;
            --highlight: #ffff00;
        }

        /* Base Styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Courier New', monospace;
            line-height: 1.6;
            color: var(--text-color);
            background-color: var(--bg-color);
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            transition: background-color 0.3s, color 0.3s;
        }

        /* Typography */
        h1, h2, h3 {
            margin: 20px 0 10px 0;
            font-weight: normal;
        }

        h1 {
            font-size: 24px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
        }

        a {
            color: var(--link-color);
            text-decoration: underline;
        }

        a:visited {
            color: var(--visited-color);
        }

        /* Layout Components */
        .header {
            text-align: center;
            margin-bottom: 40px;
            border: 1px dashed var(--border-color);
            padding: 20px;
        }

        .nav {
            margin: 20px 0;
            text-align: center;
        }

        .nav a {
            margin: 0 10px;
        }

        .theme-toggle {
            position: fixed;
            top: 10px;
            right: 10px;
            cursor: pointer;
            font-size: 20px;
            background: none;
            border: 1px solid var(--border-color);
            color: var(--text-color);
            padding: 5px 10px;
        }

        /* Search */
        .search-container {
            margin: 20px 0;
            padding: 10px;
            border: 1px solid var(--border-color);
        }

        #search-input {
            width: 100%;
            padding: 5px;
            font-family: 'Courier New', monospace;
            background-color: var(--bg-color);
            color: var(--text-color);
            border: 1px solid var(--border-color);
        }

        /* Blog Posts */
        .blog-post {
            margin: 30px 0;
            padding: 20px;
            border: 1px solid var(--border-color);
        }

        .post-meta {
            font-size: 12px;
            color: var(--text-color);
            opacity: 0.7;
            margin-bottom: 10px;
        }

        /* Tags */
        .tags {
            margin: 10px 0;
        }

        .tag {
            display: inline-block;
            padding: 2px 8px;
            margin: 2px;
            background-color: var(--tag-bg);
            border: 1px solid var(--border-color);
            font-size: 12px;
            cursor: pointer;
            text-decoration: none;
            color: var(--text-color);
        }

        .tag:hover {
            background-color: var(--highlight);
            color: #000000;
        }

        /* Utilities */
        .hidden {
            display: none;
        }

        .highlight {
            background-color: var(--highlight);
            color: #000000;
        }

        /* Code Blocks */
        pre {
            background-color: var(--code-bg);
            border: 1px solid var(--border-color);
            padding: 10px;
            overflow-x: auto;
            margin: 10px 0;
        }

        code {
            font-family: 'Courier New', monospace;
            background-color: var(--code-bg);
            padding: 2px 4px;
        }

        /* Animations (See Animation Section) */
    </style>
</head>
<body>
    <button class="theme-toggle" onclick="toggleTheme()">◐</button>

    <div class="header">
        <h1>Your Name</h1>
        <p>Software Engineer | AI Researcher</p>
    </div>

    <nav class="nav">
        <a href="#blog">[Blog]</a>
        <a href="#about">[About]</a>
        <a href="#projects">[Projects]</a>
        <a href="mailto:your.email@example.com">[Email]</a>
        <a href="https://github.com/yourusername">[GitHub]</a>
    </nav>

    <div class="search-container">
        <input type="text" id="search-input" placeholder="Search posts..." onkeyup="searchPosts()">
    </div>

    <div id="blog-posts">
        <!-- Blog posts will go here -->
    </div>

    <script>
        // JavaScript functionality (see below)
    </script>
</body>
</html>
```

### JavaScript Functionality

Add this JavaScript for search, theme toggle, and tag filtering:

```javascript
// Theme toggle
function toggleTheme() {
    const html = document.documentElement;
    const currentTheme = html.getAttribute('data-theme');
    const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
    html.setAttribute('data-theme', newTheme);
    localStorage.setItem('theme', newTheme);
}

// Load saved theme
const savedTheme = localStorage.getItem('theme') || 'light';
document.documentElement.setAttribute('data-theme', savedTheme);

// Search functionality
function searchPosts() {
    const searchTerm = document.getElementById('search-input').value.toLowerCase();
    const posts = document.querySelectorAll('.blog-post');

    posts.forEach(post => {
        const title = post.querySelector('h3').textContent.toLowerCase();
        const content = post.querySelector('p').textContent.toLowerCase();
        const tags = post.getAttribute('data-tags').toLowerCase();
        
        if (title.includes(searchTerm) || content.includes(searchTerm) || tags.includes(searchTerm)) {
            post.classList.remove('hidden');
        } else {
            post.classList.add('hidden');
        }
    });
}

// Tag filtering
function filterByTag(tag) {
    const posts = document.querySelectorAll('.blog-post');
    
    posts.forEach(post => {
        const tags = post.getAttribute('data-tags').split(',');
        if (tags.includes(tag)) {
            post.classList.remove('hidden');
        } else {
            post.classList.add('hidden');
        }
    });
}
```

## Hosting with GitHub Pages

### Step 1: Create GitHub Repository

1. Go to [github.com](https://github.com) and sign in
2. Click "New repository"
3. **Name it exactly**: `yourusername.github.io`
   - Replace `yourusername` with your actual GitHub username
   - Example: `johndoe.github.io`
4. Set to **Public**
5. Don't initialize with README

### Step 2: Upload Your Website

```bash
# Navigate to your project folder
cd /path/to/your/website

# Initialize git
git init

# Add all files
git add .

# Create first commit
git commit -m "Initial website commit"

# Add GitHub repository as remote
git remote add origin https://github.com/yourusername/yourusername.github.io.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 3: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source**, select:
   - Source: Deploy from a branch
   - Branch: main
   - Folder: / (root)
4. Click **Save**

Your site will be live at `https://yourusername.github.io` within minutes!

## Custom Domain Setup

### Step 1: Purchase a Domain

Popular registrars and approximate costs:
- **Namecheap**: $10-12/year for .com
- **Google Domains**: $12/year for .com
- **Cloudflare**: At-cost pricing (~$8-9/year)
- **Porkbun**: Often has deals, ~$8-10/year

### Step 2: Create CNAME File

In your repository, create a file named `CNAME` (no extension):

```bash
echo "yourdomain.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push
```

### Step 3: Configure DNS

Add these DNS records at your domain registrar:

#### A Records (for root domain):
```
Type: A    Name: @    Value: 185.199.108.153
Type: A    Name: @    Value: 185.199.109.153
Type: A    Name: @    Value: 185.199.110.153
Type: A    Name: @    Value: 185.199.111.153
```

#### CNAME Record (for www):
```
Type: CNAME    Name: www    Value: yourusername.github.io.
```

### Step 4: Enable HTTPS

After DNS propagates (usually 1 hour):
1. Go to Settings → Pages in your repository
2. Check **Enforce HTTPS**

## Adding Retro Animations

### 1. Blinking Cursor

Add to CSS:
```css
.blink-cursor::after {
    content: '_';
    animation: blink 1s infinite;
}

@keyframes blink {
    0%, 49% { opacity: 1; }
    50%, 100% { opacity: 0; }
}
```

Use in HTML:
```html
<h1>Your Name<span class="blink-cursor"></span></h1>
```

### 2. Typewriter Effect

Add to CSS:
```css
.typewriter {
    overflow: hidden;
    white-space: nowrap;
    border-right: 2px solid;
    width: 0;
    animation: typing 3s steps(30) forwards,
               cursor-blink 0.75s infinite;
}

@keyframes typing {
    from { width: 0; }
    to { width: 100%; }
}

@keyframes cursor-blink {
    from, to { border-color: transparent; }
    50% { border-color: black; }
}
```

### 3. Loading Animation

Add to CSS:
```css
.loading::after {
    content: '.';
    animation: dots 1.5s steps(4, end) infinite;
}

@keyframes dots {
    0%, 20% { content: '.'; }
    40% { content: '..'; }
    60% { content: '...'; }
    80%, 100% { content: ''; }
}
```

Use for search:
```javascript
function searchPosts() {
    const searchStatus = document.createElement('span');
    searchStatus.innerHTML = 'Searching<span class="loading"></span>';
    // ... rest of search logic
}
```

### 4. Glitch Effect

Add to CSS:
```css
.glitch {
    position: relative;
}

.glitch:hover::before,
.glitch:hover::after {
    content: attr(data-text);
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

.glitch:hover::before {
    animation: glitch-1 0.3s infinite;
    color: #00ff00;
    z-index: -1;
}

.glitch:hover::after {
    animation: glitch-2 0.3s infinite;
    color: #ff00ff;
    z-index: -2;
}

@keyframes glitch-1 {
    0%, 100% { clip-path: inset(40% 0 60% 0); }
    20% { clip-path: inset(20% 0 30% 0); transform: translate(-2px, 2px); }
    40% { clip-path: inset(60% 0 20% 0); transform: translate(2px, -2px); }
}

@keyframes glitch-2 {
    0%, 100% { clip-path: inset(60% 0 40% 0); }
    20% { clip-path: inset(30% 0 20% 0); transform: translate(2px, -2px); }
    40% { clip-path: inset(20% 0 60% 0); transform: translate(-2px, 2px); }
}
```

### 5. Accessibility for Animations

Always include:
```css
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

## Deployment Workflow

### Initial Setup
1. Create website locally
2. Test in browser
3. Initialize git repository
4. Push to GitHub
5. Enable GitHub Pages
6. Configure custom domain
7. Wait for DNS propagation

### Updating Content

```bash
# Make changes to your files
# Then:
git add .
git commit -m "Add new blog post about transformers"
git push

# Changes appear within 1-2 minutes
```

### Blog Post Template

```html
<article class="blog-post" data-tags="tag1,tag2,tag3">
    <h3><a href="#">Post Title</a></h3>
    <div class="post-meta">2024-03-15 | 5 min read</div>
    <p>Post excerpt or summary...</p>
    <div class="tags">
        <a href="#" class="tag" onclick="filterByTag('tag1'); return false;">tag1</a>
        <a href="#" class="tag" onclick="filterByTag('tag2'); return false;">tag2</a>
        <a href="#" class="tag" onclick="filterByTag('tag3'); return false;">tag3</a>
    </div>
</article>
```

## Maintenance & Best Practices

### Performance
- Keep images under 200KB
- Use WebP format for images
- Minimize external dependencies
- Leverage browser caching

### SEO
- Add meta descriptions
- Use semantic HTML
- Create a sitemap.xml
- Submit to Google Search Console

### Content Management
- Use consistent naming for files
- Keep a local backup
- Tag posts consistently
- Update regularly

### Security
- Always use HTTPS
- Keep email obscured or use contact form
- Regular backups via git

### Monitoring
- Check Google Analytics (optional)
- Monitor loading speed
- Test on multiple devices
- Verify HTTPS certificate renewal

## Troubleshooting

### Common Issues

**Site not loading?**
- Check repository name matches `username.github.io`
- Verify GitHub Pages is enabled
- Wait 10 minutes for initial deployment

**Custom domain not working?**
- Check DNS propagation: `dig yourdomain.com`
- Verify CNAME file contains correct domain
- Ensure DNS records are exactly as specified

**HTTPS errors?**
- Wait 24 hours after DNS setup
- Check "Enforce HTTPS" is enabled
- Clear browser cache

**404 errors?**
- Ensure `index.html` is in repository root
- Check file names are case-sensitive
- Verify branches match (main vs master)

## Next Steps

1. **Enhance Content**
   - Add more blog posts
   - Create individual post pages
   - Add RSS feed

2. **Improve Features**
   - Add comment system (Utterances)
   - Implement full-text search
   - Add reading time calculator

3. **Analytics**
   - Add privacy-friendly analytics
   - Track popular posts
   - Monitor site performance

4. **Automation**
   - Set up GitHub Actions
   - Automate sitemap generation
   - Add link checking

## Resources

- [GitHub Pages Documentation](https://docs.github.com/pages)
- [MDN Web Docs](https://developer.mozilla.org)
- [Can I Use](https://caniuse.com) - Browser compatibility
- [PageSpeed Insights](https://pagespeed.web.dev) - Performance testing

---

This guide provides everything needed to build, deploy, and maintain an early-internet style technical blog with modern functionality. The aesthetic captures the nostalgia of the early web while providing a clean, functional platform for technical writing.