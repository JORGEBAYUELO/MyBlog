---
title: How I Built My Automated Dev Blog with Obsidian, Hugo, and GitHub Pages
date: 2025-04-07
draft: false
cover: /stacktools_cover.jpg
---
## Introduction:
I’ve always wanted a place where I could document my journey into tech, especially as I transition from being a technical artist to a DevOps engineer. But I didn’t just want _any_ blog. I wanted something **clean**, **fast**, and most importantly, **automated**. That way, I could focus on writing, not fiddling with deployment.

So I decided to build my blog using a stack that aligned with the tools I love:  
📝 Obsidian for writing,  
⚙️ Hugo for static site generation, and  
🚀 GitHub Pages for free and automated deployment.

This is the story of how I put it all together, and how it’s already changed the way I share what I learn.
## Choosing the Tools:
The idea to combine **Obsidian and Hugo** came after watching a video from [**NetworkChuck** ](https://www.youtube.com/@NetworkChuck)on YouTube. His approach to turning Markdown notes into a blog really caught my attention. I took inspiration from there, but I wanted to make the workflow completely my own.
So I made several changes:
- I customized the folder structure to better match my writing habits.
- I set up an **automated CI/CD workflow** with GitHub Actions.
- I also tailored the deployment and theming to better reflect _me_.
Using [Obsidian](https://obsidian.md/) was a no-brainer since I already use it for notes. It made sense to extend it into my blog writing tool.

For the static site generator, I picked [Hugo](https://gohugo.io/) because it’s super fast and very Git-friendly. Lastly, hosting it on **GitHub Pages** gave me free hosting with my own domain: [jorgebayuelo.blog](https://jorgebayuelo.blog).
### The Workflow
Here’s what it looks like:
- I write my blog posts in Obsidian using a template powered by the Templater plugin.
- Once I’m ready, I push the changes to GitHub.
- A GitHub Actions workflow kicks in, builds the Hugo site, and deploys it to the `gh-pages` branch.
- That branch is linked to GitHub Pages and automatically updates the live site.
So I get a **fully automated blog**, with no manual deployment steps, just write, commit, and push. 💨
### Styling the Blog
I chose the **Terminal** theme because of its simplicity and retro vibe. But I wanted to customize the colors using the beautiful [Catppuccin](https://github.com/catppuccin) palette.

This required diving into the theme’s CSS and tweaking it to match my personal style, which brings us to one of the tricky parts…
## Challenges and How I Solved Them:
### 1. Git Submodules Are… Weird
Installing the theme as a Git submodule was convenient until I realized I couldn't directly modify and commit changes to it. Any local changes to the theme wouldn’t stick or sync easily.

**Fix:** I **forked** the Terminal theme repo to my own GitHub account, then **cloned it using SSH**. This gave me full control over the theme. Now I can modify it freely, commit changes locally, and push to _my own_ theme repository.
### 2. Broken Deployments with GitHub Actions
My deployments kept failing with odd `403` errors and messages like “You deploy from main to main.”

**Fix:** I switched to deploying from `main` to a dedicated `gh-pages` branch, and I updated the repository’s settings to give **GitHub Actions write permissions** under `Settings > Actions > General`.
### 3. Custom Domain Not Resolving
After adding my domain, I was still seeing 404 errors. Turns out my Hugo config was missing the proper base URL.

**Fix:** I added this to my `config.toml`:
```toml
baseURL = "https://jorgebayuelo.blog"
```
That instantly resolved the issue. No more 404s.
### 4. Images and Assets
At first, my images weren’t showing up in the blog. I learned Hugo has expectations around where and how images are placed and linked.

**Fix:** I adjusted my Obsidian vault structure and used Hugo’s content bundle format. Now images placed alongside each post render correctly on the site.
## Conclusion:
This blog isn’t just a publishing platform, it’s an **extension of my learning environment**. Building it taught me how to tie together a static site generator, Git workflows, CI/CD automation, and even DNS configuration.

It’s now a smooth, personalized workflow that lets me focus on what matters most: **sharing my projects, insights, and journey** with the world.