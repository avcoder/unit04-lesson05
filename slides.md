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
- [ ] Prevent routes/displays based on authenticated users

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
   - we've done CRUD, and showing messages
   - today we'll implement register and login 
- Reminder: Lab tomorrow
- Today will show both mid-term interpretations:
   1. users can see/edit their own notes
   2. users can see other users notes but can only edit their own 
- after today's class you'll have everything you need for midterm
- Moving API Design to lesson 08

---
transition: slide-left
---

# Register (pg.1)

- I've already included register.ejs and login.ejs in foodtruck-tempate repo
- Add route in router.js to eventually display register.ejs
  ```js
  router.get("/register", userController.registerForm);
  ```
- in /controllers/userController.js
  ```js
  const registerForm = async (req, res) => {
    res.render("register", { title: "Register" });
  };
  ```
- Examine register.ejs form verify action and method

---
transition: slide-left
---

# Register (pg.2)

- Add POST /register route to router.js
  ```js
  router.post("/register", userController.register);
  ```
- in /controllers/userController
  ```js
  import userHandler from "../handlers/userHandler.js";

  const register = async (req, res) => {
    const callback = (err, newUser) => {
      if (err) {
        res.redirect("/register");
      } else {
        res.redirect("/login");
      }
    };

    userHandler.register({
      username: req.body.username,
      password: req.body.password,
      callback,
    });
  };
  ```

---
transition: slide-left
---

# Register (pg.2)

- in /handlers/userHandler.js
  ```js
  import User from "../models/userModel.js";

  const register = async ({ username, password, callback }) => {
    await User.register(new User({ username }), password, callback);
  };

  export default {
    register,
  };
  ```

---
transition: slide-left
---

# Register (pg.3)

- in /models/userModel.js
  ```js
  import mongoose from "mongoose";
  import validator from "validator";
  import plm from "passport-local-mongoose";

  const userSchema = mongoose.Schema({
    username: {
      type: String,
      required: [true, "Please provide an email address"],
      unique: true,
      lowercase: true,
      trim: true,
      validate: [validator.isEmail, "Invalid email"],
    },
  });
  ```

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

# Show

- ensure 

---
transition: slide-left
---

# Create 

- create 

---
transition: slide-left
layout: two-cols
---

# Lab tomorrow
Option: Choose a Getting Started Activity, OR ask questions AMA (ex: error handling review)

Do at least 1 "Getting Started" from:
  - [Socket io](https://socket.io/get-started/chat)
  - [AI LLM chat model](https://js.langchain.com/docs/tutorials/llm_chain)
  - [Snipcart](https://docs.snipcart.com/v3/)
  - [Nest JS](https://docs.nestjs.com/first-steps)
  - [Next JS](https://nextjs.org/docs/app/getting-started/installation)
  - [Netlify Serverless Functions](https://docs.netlify.com/functions/get-started/?fn-language=js)

::right::

# 
  - [Go](https://go.dev/doc/tutorial/getting-started)
  - [GraphQL](https://graphql.org/graphql-js/)
  - [Electron](https://www.electronjs.org/docs/latest/tutorial/tutorial-first-app)
  - [Astro](https://docs.astro.build/en/tutorial/0-introduction/)
  - [Twilio](https://www.twilio.com/docs/messaging/quickstart)
  - [Tauri](https://tauri.app/start/)