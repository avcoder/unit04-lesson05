---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Full-Stack Development

Full-Stack Development - part 5/8

- [ ] Register User
- [ ] Login Functionality
- [ ] API Design

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Recap

- Foodtruck template Repo: https://github.com/avcoder/foodtruck-template
- Q: For mid-term, can I do a [whatever] app instead of a note-taking app
- A: It can only be a note-taking app.
- Lab tmrw

---
transition: slide-left
---

# Serve static files

Add upload photo functionality

- `npm i jimp multer uuid`
- update truckModel.js to include `photo: String,`
- update router.js to include an `truckController.upload` and `truckController.resize`
- in truckController.js, create `upload` and `resize` functions

---
transition: slide-left
---

# Show Alerts via connect-flash

- connect-flash is used to show success messages, error messages, info etc.
- How does it work?
  - if you wish to pass any message, use: `req.flash()`
  - pass in a type of message, and an actual message
  - connect-flash will then stick that info in the next request object (via sessions)
  - That info will then clean up after itself after that 1st request
  
---
transition: slide-left
---

# Exercise: Show Alerts via connect-flash

- Goal: modify the code to show success and error messages where appropriate.  Use bootstrap [Alert component](https://getbootstrap.com/docs/5.3/components/alerts/) to do so and modify any ejs pages to display them.

- in app.js:
  ```js
  import flash from "connect-flash";
  import { notFound, flashValidationErrors } from "./handlers/errorHandlers.js";

  app.use(flash());

  res.locals.flashes = req.flash(); // flash messages (ex: succes`s, error, info)

  app.use(flashValidationErrors); // flash validation errors
  ```
- create `flashValidationErrors()` in errorHandlers.js
- in truckController.js, insert `req.flash("success", `/${truck.slug} added successfully!`


---
layout: image-right
transition: slide-left
image: /assets/matt.png
backgroundSize: 400px 320px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:

- 🧼 [Background Removal Tool](https://huggingface.co/spaces/Xenova/remove-background-web)
- 🎒 [Self-hosted Apps](https://selfhosted.libhunt.com/)
- 😵‍💫 [Brutalist Report](https://brutalist.report/)
- 📝 [Markdown Notes](https://github.com/orgs/community/discussions/16925)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

<!--
- take attendance
-->

---
transition: slide-left
---

# Show a google map based off lat/lng coordinates

- We need to adjust our schema to include location etc.
- let's move our choices to truckController as a const
- adjust our addTruck form and editTruck form to include location
- add autocomplete functionality for address
- add slug page
- make static google map based off our lat/lng
- ensure slug is unique

---
transition: slide-left
---

# Create tags page

- create `getTags` route
- add `.getAllTags()` method to Truck schema
- make clicking each tag filter and display corresponding food trucks with tag

---
transition: slide-left
---

# Lab tomorrow

Do at least 1 "Getting Started" from:
  - [Socket io](https://socket.io/get-started/chat)
  - [AI LLM chat model](https://js.langchain.com/docs/tutorials/llm_chain)
  - [Snipcart](https://docs.snipcart.com/v3/)
  - [Nest JS](https://docs.nestjs.com/first-steps)
  - [Next JS](https://nextjs.org/docs/app/getting-started/installation)
  - [Netlify Serverless Functions](https://docs.netlify.com/functions/get-started/?fn-language=js)
  - [Go](https://go.dev/doc/tutorial/getting-started)
  - [GraphQL](https://graphql.org/graphql-js/)
  - [Electron](https://www.electronjs.org/docs/latest/tutorial/tutorial-first-app)
  - [Astro](https://docs.astro.build/en/tutorial/0-introduction/)