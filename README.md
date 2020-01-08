# JavaScript Project Instructions

<!-- toc -->

- [Overview](#overview)
- [Objectives](#objectives)
- [Project Deliverables](#project-deliverables)
- [Technical and Complexity Requirements](#technical-and-complexity-requirements)
- [What to Expect in Your Project Review](#what-to-expect-in-your-project-review)
- [Example Projects](#example-projects)

<!-- tocstop -->

## <a id="overview"></a>Overview

Welcome to your JavaScript Project!

You are going build a Single Page Application (**SPA**). Your frontend will be built with HTML, CSS, and JavaScript. Your frontend will communicate with a backend API that you'll build with Ruby and Rails. This is a really exciting moment - the whole course up until this point is coming together!

## <a id="objectives"></a>Objectives

Your goals with this project:

- Design and architect features across frontend and backend
- Integrate JavaScript and Rails
- Debug issues in small- to medium-sized projects
- Build and iterate on a project MVP
- Communicate in a technical environment

## <a id="project-deliverables"></a>Project Deliverables

In order to schedule your Project Review, you must submit:

- A link to your project repo, with code for your Rails backend and HTML / CSS / JavaScript frontend.
- A `README.md` file describing your application
- A **Blog Post** about your application
- A 2-4 minute **Video Demo** introducing your application

Read below for more details about the technical requirements for your project. See the section "Communicating About Your Project" in the [Project Planning and Tips](https://github.com/learn-co-curriculum/js-spa-project-instructions/blob/master/project-planning-tips.md) document for more guidance on communicating about your project.

As always, your project must be your own work. For more details about the rules of the road, see [Project Rules of the Road](https://github.com/learn-co-curriculum/js-spa-project-instructions/blob/master/project-rules-of-the-road.md).

## <a id="technical-and-complexity-requirements"></a>Technical and Complexity Requirements

In order to demonstrate your proficiency with what you've learned about web development with JavaScript, here are the requirements for your project. You should view these guidelines as a minimum bar for the features you include in your application. It's your project, and you are encouraged to go above and beyond these requirements.

1. The application must be an HTML, CSS, and JavaScript frontend with a Rails API backend. All interactions between the client and the server must be handled asynchronously (AJAX) and use JSON as the communication format.

2. The JavaScript application must use Object Oriented JavaScript (classes) to encapsulate related data and behavior.

3. The domain model served by the Rails backend must include a resource with at least one has-many relationship. For example, if you were building an Instagram clone, you might display a list of photos with associated comments.

4. The backend and frontend must collaborate to demonstrate Client-Server Communication. Your application should have at least 3 AJAX calls, covering at least 2 of Create, Read, Update, and Delete (CRUD). Your client-side JavaScript code must use `fetch` with the appropriate HTTP verb, and your Rails API should use RESTful conventions.

Within these constraints, there is a huge variety of applications that you might build. Take some time to brainstorm the application you'd like to build. Take a look at the [example projects](#example-projects) at the bottom of this page for inspiration. You should build something that you are excited to talk about. That means being excited about the features you build and the technology that you use.

If you aren't sure about whether the domain or features you are planning to build will meet the technical requirements, you should reach out to an instructor for guidance. [Read more about instructor guidance below](#instructor-guidance).

### Best Practices

You are encouraged to follow development best practices while you are building your application.

#### JavaScript

- [ ] Use classes and functions to organize your code into reusable pieces.
- [ ] Translate JSON responses into JavaScript model objects using ES6 class or constructor function syntax.
- [ ] Use ES6 features when appropriate (e.g. arrow functions, `let` & `const`, rest and spread syntax).

#### Rails

- [ ] Follow Rails MVC and RESTful conventions. That means, for example, that a request `GET /puppies` ought to be handled by the `PuppiesController`, fetch puppies from the database using a `Puppy` Active Record model, and return a list of puppies as JSON.
- [ ] Well-named variables and methods
- [ ] Short, single-purpose methods

#### Git

- [ ] Aim for a large number of small commits - commit frequently!
- [ ] Add meaningful messages to your commits. When you look back at your commits with `git log`, the messages should describe each change.
- [ ] Don't include changes in a commit that aren't related to the commit message

### Suggested Project Structure

You must submit a link to the repo with the code for your project. There is no requirement for how you decide to structure the code within that repo, but in the past, students have had success using a structure like:

```txt
javascript-project/
  backend/
    app/
    (...other rails files and folders)
  frontend/
    index.html
    style.css
    index.js
  README.md
```

For more about setting up your backend, you can reference the lesson on [Creating a Rails API from Scratch](https://github.com/learn-co-curriculum/js-rails-as-api-creating-a-rails-api-from-scratch).

## <a id="what-to-expect-in-your-project-review"></a>What To Expect In Your Project Review

Review the [What to Expect in Project Reviews](https://github.com/learn-co-curriculum/js-spa-project-instructions/blob/master/what-to-expect-in-project-reviews.md) document for general guidance on what to expect in the Project Review.

### What should you be prepared for in Project Review?

During your project review, be prepared to:

1. Explain your code from execution point to exit point. Use the best technical vocabulary you can.
2. Live code. This could be refactoring, adding a new feature, or both.
3. Answer questions about your knowledge of _JavaScript Fundamentals_.

In particular, the JavaScript Fundamentals concepts your reviewer may ask about include:

- variables
- data structures
- functions
- hoisting
- scope
- context
- `this`
- closures
- ES6 syntax
- `let`, `const`
- arrow functions

### Learning Goals

These are the skills and knowledge that you should aim to demonstrate through the project review.

- Explain how Rails routes a request to a controller and method based on the URL and HTTP verb
- Use `render json:` to render serialized JSON
- Select, Create, and Modify DOM nodes
- Attach listeners to DOM nodes to respond to user interaction
- Use `preventDefault` to control form submit behavior
- Use `fetch` with 'GET', 'POST', 'PATCH' & 'DELETE' HTTP methods
- Create a JavaScript object with ES6 class syntax
- Instantiate JavaScript objects and call methods on them.

## <a id="example-projects"></a>Example Projects

- [Beat Machine](https://beat-machine.com/)
- [Bounce Game](http://bounce-123.s3-website-us-east-1.amazonaws.com/)
- [Words With Nerds™](https://wordswithnerds.herokuapp.com/)
- [Remixer](https://remixer-v2.firebaseapp.com/)
- [Keyboard Karaoke](https://keyboard-karaoke.herokuapp.com/)
