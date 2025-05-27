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
- [ ] Prevent routes/displays for unauthenticated users

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
   - today we'll implement register and login; protecting our routes if user is not authenticated
- Reminder: Lab tomorrow
- Today will show both mid-term interpretations:
   1. users can see/edit their own notes
   2. users can see other users notes but can only edit their own 
- after today's class you'll have everything you need for midterm
- Midterm Q: Should I deploy my app on render.com or elsewhere?
   - A: No deployment necessary for midterm (we have a Deployment unit for that)
   - Just submit your Github repo including README.md, email me privately your .env file
   - I will git clone your repo then `npm i` on my ends
- Moving API Design to lesson 08

---
transition: slide-left
---

# Register (pg.1)

- I've already included register.ejs in foodtruck-template repo
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
transition: slide-left
---

# Register (pg.4)

- `router.post("/register", userController.validateRegister, userController.register);`
- in userController.js
  ```js
  const validateRegister = [
    body("username").notEmpty().withMessage("Email address is required"),
    body("username").isEmail().withMessage("Please provide a valid email"),
    body("password")
      .isLength({ min: 6 })
      .withMessage("Password must be at least 6 characters"),
    body("confirm-password")
      .isLength({ min: 6 })
      .withMessage("Confirm Password must be at least 6 characters"),
    body("confirm-password").custom((value, { req }) => { return value === req.body.password; })
      .withMessage("Password does not match Confirm Password"),
    (req, res, next) => {
      const errors = validationResult(req);
      if (!errors.isEmpty()) {
        req.flash("danger", errors.errors.map((err) => err.msg).join(". "));
        res.render("register", { title: "Register", flashes: req.flash() });
      } else {
        next();
      }
    },
  ];
  ```

<!-- 
- app.js, after `res.locals.currentPath = req.path`
- const errors = req.flash("error");
- if (errors.length) {
-   req.flash("danger", errors)
- }
-->
---
transition: slide-left
---

# Login (pg.1)

- I've already included login.ejs in foodtruck-tempate repo
- Add route in router.js to eventually display login.ejs
  ```js
  router.get("/login", userController.loginForm);
  ```
- in /controllers/userController.js
  ```js
  const loginForm = async (req, res) => {
    res.render("login", { title: "Login" });
  };
  ```
- Examine login.ejs form verify action and method

---
transition: slide-left
---

# Login (pg.2)

- Add POST /login route to router.js
  ```js
  router.post("/login", authController.login);
  ```
- create /controllers/authController.js
  ```js
  import passport from "passport";

  const login = passport.authenticate("local", {
    successRedirect: "/",
    failureRedirect: "/login",
    failureMessage: "⚠️ Invalid Login",
  });

  export default {
    login,
  };
  ```
- we've already implemented passport package in our app.js, passport.js

---
transition: slide-left
---

# Logout

- in router.js
  ```js
  router.get("/logout", authController.logout);
  ```
- in authController.js
  ```js
  const logout = (req, res, next) => {
    req.logout((err) => {
      if (err) {
        return next(err);
      }
    });
    req.flash("success", "👋 You have logged out");
    res.redirect("/");
  };
  ```

---
layout: image-right
transition: slide-left
image: /assets/dodds.png
backgroundSize: 400px 250px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:

- 🎂 [Happy 30th JS](https://deno.com/blog/history-of-javascript)
- ⚙️ [How Promises Work](https://www.deepintodev.com/blog/how-promises-work-in-javascript)
- 👉 [JS this](https://piccalil.li/blog/javascript-when-is-this/)
- 🥊 [Fighting the JS trademark](https://deno.com/blog/deno-v-oracle3)
- 🛑 [Stop Lying to your Users](https://www.epicweb.dev/stop-lying-to-your-users)

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

# Protect Routes (pg.1)

- Now that we have login and register working, we can now protect our routes, edit/delete buttons, from being viewed by unauthenticated users 
- in router.js
  ```js
  router.get("/trucks", authController.isAuthenticated, catchErrors(truckController.getTrucks));

  router.get("/add", authController.isAuthenticated, truckController.addTruck);

  router.get("/trucks/:id/edit", authController.isAuthenticated, catchErrors(truckController.editTruck));

  router.post("/trucks/:id/edit",  authController.isAuthenticated, ...

  router.delete("/trucks/:id", authController.isAuthenticated, truckController.deleteTruck);

  router.get("/logout", authController.isAuthenticated, authController.logout);
  ```

---
transition: slide-left
---

# Protect Routes (pg.2) 

- Hide any HTML/EJS content via:
  ```html
  <% if (user) { %>
    <li class="nav-item">
      etc...
  <% } %>

  <% if (user) { %>
    <th scope="col">Edit</th>
    <th scope="col">Delete</th>
  <% } %>

  <% user && u.menu.map( menuItem => { %>
    <li class="nav-item">
    etc...
  <% } %>
  ```
- Where did user come from? (see app.js under res.locals section)

---
transition: slide-left
---

# Exercise: Protect any content

- Go through your app and protect any html/ejs content from being viewed by unauthenticated users
  - Use previous slide to help
  - Can use foodtruck app, or your own note-taking app

---
transition: slide-left
---

# Ensuring Validation Alerts work

- Let's verify that any validation errors are showing as alerts
- If we have remaining time, you can start/continue working on your midterm.
   - Feel free to ask me to take a look/ask any questions

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
  - [Puppeteer](https://pptr.dev/guides/what-is-puppeteer)
  - [Go](https://go.dev/doc/tutorial/getting-started)
  - [Ruby on Rails](https://guides.rubyonrails.org/getting_started.html)
  - [GraphQL](https://graphql.org/graphql-js/)

::right::

# 
  - [Electron](https://www.electronjs.org/docs/latest/tutorial/tutorial-first-app)
  - [Astro](https://docs.astro.build/en/tutorial/0-introduction/)
  - [Twilio](https://www.twilio.com/docs/messaging/quickstart)
  - [Tauri](https://tauri.app/start/)
  - [11ty](https://www.11ty.dev/docs/)
  - [Tailwind CSS](https://tailwindcss.com/docs/installation/using-vite)
  - [Typescript](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
  - [Vue.js](https://vuejs.org/guide/introduction)
  - [Svelte](https://svelte.dev/tutorial/svelte/welcome-to-svelte)
  - [Angular](https://angular.dev/tutorials)
  - [GSAP](https://gsap.com/resources/get-started)
  - [Three.js](https://threejs.org/manual/#en/creating-a-scene)
  - [D3](https://d3js.org/getting-started)
  - or any other tech you wish to learn based off job postings