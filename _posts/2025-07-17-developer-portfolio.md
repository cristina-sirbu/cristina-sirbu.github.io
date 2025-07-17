---
title: "How I Built My Blog"
subtitle: "How it's made"
date: 2025-07-17
layout: post
tags: [jekyll, github-pages, career, developer-portfolio]
background: '/img/posts/how-i-built-my-dev-portofolio.jpg'
---

Creating a personal developer portfolio can be surprisingly tricky, especially if you want it to reflect your personality and values. I wanted something simple, elegant and low-maintenance, but still flexible enough to grow with me. In this post, I will walk you through how I built this site using **GitHub Pages**.

---

## Step 1: Choosing a Theme

After exploring several themes (maybe a bit too many 😅), I picked [Clean Blog Jekyll](https://startbootstrap.github.io/startbootstrap-clean-blog/) because:

- It had a simple, readable layout
- It already had support for posts and pages
- It looked good on mobile with minimal tweaking

You can either [fork the theme](https://github.com/StartBootstrap/startbootstrap-clean-blog) or clone it:

```bash
git clone https://github.com/StartBootstrap/startbootstrap-clean-blog.git my-site
cd my-site
```

---

## Step 2: Make it your own

Start by customizing the config. I updated `_config.yml` with my name, GitHub handle and site description. This file is the brain of a Jekyll site.

Set up GitHub Pages. I pushed the project to `cristina-sirbu.github.io` repository on Github, enabled Github Pages in the repo settings and the site was live in minutes. ([documentation](https://docs.github.com/en/pages/quickstart))

```shell
git remote set-url origin https://github.com/cristina-sirbu/cristina-sirbu.github.io.git
git push -u origin main
```

To tweaked the design and make it my own, I changed the background image for each page and started writing the *About Me* and *CV* pages.

---

## Step 3: Set up Contact Me page

To allow visitors to contact you directly without exposing your email, I used [Formspree](https://formspree.io/). It is a simple, spam-protected way to add a contact form.

Create a file like contact.html in your root or _pages/ directory:

```html
---
layout: page
title: "Contact"
permalink: /contact/
background: '/img/bg-contact.jpg'
---

<h2>Let's get in touch</h2>
<form action="https://formspree.io/f/your_form_id" method="POST">
  <label for="name">Name</label>
  <input type="text" name="name" required>

  <label for="email">Email</label>
  <input type="email" name="email" required>

  <label for="message">Message</label>
  <textarea name="message" rows="6" required></textarea>

  <button type="submit">Send</button>
</form>
```

Go to [formspree.io](https://formspree.io/). Sign up and create a new form. Copy the form action URL (e.g., https://formspree.io/f/mabcdxyz and paste it into your `<form action="...">` tag.

---

## Step 4: Start writing

My posts live in `_posts/` and use Markdown. Writing and versioning content as code is a huge productivity boost.

---

## Reflections

My adivce to my previous self is to not spend too much time with the details (picking the theme, the images, etc.). Just choose something knowing that it will change in the future. Don't loose focus. The main idea is to share the knowledge. So ...

Happy writing! 🙂
