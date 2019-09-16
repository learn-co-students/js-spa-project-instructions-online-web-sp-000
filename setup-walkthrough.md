# JavaScript Project Setup Walkthrough

For your project, you will be building a Ruby on Rails backend and JavaScript frontend. Two parts of the application talking to each other means there will be a lot of moving parts! This document will walk you through setting up your project.

Since you want to keep our frontend and backend code separate, you will have two separate top-level folders in our project, for the Rails app and the JavaScript app. When you finish this walkthrough, your file structure will look like:

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

## Creating the top-level folder

Once you have planned your project, choose a location on your computer where you'd like to save your project. Move to that location in the terminal with `cd`. For this walkthrough, we'll call the project 'walkthrough-project'. You should choose a name that fits your project - something other than 'walkthrough-project'.

```bash
mkdir walkthrough-project
cd walkthrough-project
```

## Setting Up the Backend Rails API

When you create a new Rails application, Rails provides you with a ton of stuff that you will not need in order to build an API. Think of the entire `ActionView` library (all the view helper methods like `form_for`), sessions and cookies handling, etc.

[Rails provides a way](http://edgeguides.rubyonrails.org/api_app.html) to set up a project for an API. It will include what you need for an API, and exclude the unnecessary extras. Recall the lesson on [Creating a Rails API from Scratch](https://github.com/learn-co-curriculum/js-rails-as-api-creating-a-rails-api-from-scratch).

### Create a new Rails API project

In your terminal, enter:

```bash
rails new walkthrough-backend --api --database=postgresql
```

For your project, `rails new <my_app_name> --api`. _(Replace `<my_app_name>` with the actual name of your project)_

This will generate a new rails project using Postgresql as the database. We specify the `--api` flag so rails knows to set this up as an API.

> Note: _Postgresql_ is a type of SQL database. We'll want to use Postgres so that it's easier to deploy the application on Heroku (which does not support SQLite, the default database for Rails). You should have installed Postgres when you initially set up your machine, if you finished the [OSX manual set up instructions](https://help.learn.co/en/articles/900121-mac-osx-manual-environment-set-up). **Check that you have Postgres installed and running on your computer**. Usually, you can look for the elephant icon at the top of your screen. From the command line, you can check that Postgres is installed with `postgres --version`. You can also check that Postgres is running with `ps -A | grep postgres`.  If you have not yet set up Postgres, [you can download it here](http://postgresapp.com/)

The `config.api_only = true` option found in `config/application.rb` tells Rails that our app will be an **API only**. In other words, our API **will not generate any HTML** - instead, it will return JSON. The frontend is responsible for fetching the data as JSON and formatting it to show to the user. Read [this](https://www.w3schools.com/js/js_json_intro.asp) if you want to review what JSON is and why we use it.

### Configure CORS

> [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a security feature that only allows API calls from known origins. For example, if someone tried to use some malicious JavaScript to steal your bank information and your bank allowed API calls from anywhere, this could be a bad news bears™️ situation.
>
> Let's say your bank is hosted on `https://bankofamerica.com`. If a clever hacker tried to impersonate you and send a request with an _origin_ of `https://couponvirus.org`, then ideally, your bank would reject requests from unknown _origins_ such as `couponvirus.org` and only allow requests from trusted origins or domains like `bankofamerica.com`
>
> When developing your application, the backend and frontend may be served from different origins. Turning on CORS in your backend will allow your frontend to make requests, even though it's on a different origin.

- `cd` into the new project folder you just created.

- Navigate to your Gemfile and uncomment `gem 'rack-cors'`. This will allow us to setup Cross Origin Resource Sharing (CORS) in our API. You can read more about CORS [here](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

- Run `bundle install`

- Inside of `config/initializers/cors.rb` uncomment the following code:

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

(This snippet is from the [documentation for the rack-cors gem](https://github.com/cyu/rack-cors).)

Inside the `allow` block, `origins '*'` means we are allowing requests from **all** origins and are allowing `[:get, :post, :patch, :delete]` requests to the API. Read [this](https://www.w3schools.com/tags/ref_httpmethods.asp) if you need a refresher on HTTP methods.

For now, leave the origins open. Later on, you can change this to only allow requests from the address of the frontend repo––localhost:8000 or `www.myapp.com` for example.

### Models, Seeds, Routes & Controller

Now that you have the initial Rails app configured, you should add your first model, routes, controller, and seed data. You should test your code along the way using the Rails console (`rails c`). When you have your controller and routes set up, you can see them in action by running `rails s` and opening your app in the browser.

# Setting Up the Frontend (Client) Application

In Rails, the framework is very _opinionated_. That is, it has a lot of opinions about how your code should be structured - which files and which methods should go where. The same rules don't apply for your JavaScript app. There is no 'one right way' to structure your code. In this project, we are not using a frontend framework, and the design decisions will be up to you.

The walkthrough will demonstrate one reasonable way to structure the application. You can structure your code in a similar pattern, but should not feel tied to exactly these files or organization.

## Initial Setup

- Make a new directory in your top-level project for your frontend and `cd` into it.

```bash
mkdir walkthrough-frontend
cd walkthrough-frontend
```

> Tip: you can open up a new tab in terminal with `command + t` if you'd like to have your rails server up and running in another tab.

- In the new folder, create a single HTML page for your application, and a folder to hold your JavaScript files.

```bash
touch index.html
mkdir src
touch src/index.js
```

- Start with a single JavaScript file. Later on, you can split your code into multiple files for organization.

```bash
touch src/index.js
```

In `index.html`, you need to add some HTML. Text editors will often have a shortcut for creating a blank HTML document. If you use the Atom editor, you can begin typing "html" and then press tab to auto-generate the starter HTML.

```html

```

## Example Feature (Updating a note)

We want to create the following features:

> As a user, when the page loads I should see a list of notes. Next to the title of each note will be a button to edit that note.

> As a user, when I click the edit button, I will see a form with the values of that note in the input fields. I can make changes, click 'Save Note' and see the changes reflected in the list of notes.

Delivering these features will involve several steps and we will want to be sure **to work iteratively**. We will make it work and then make it better.

### Step 1: Fetching Notes

The first step is getting the list of notes to show up on the page. Translating that to more technical language, we need to:

1 - Fire off an AJAX fetch request to the index route (i.e GET '/notes')

2 - Use the response JSON to append elements to the DOM.

Let's be sure not to overcomplicate our approach, we can (and will) refactor later. At the same time, we don't want to be debugging the routes in our Rails application trying to figure why our server isn't responding when it turns out we forgot to include a script tag linking `src/index.js` in `index.html`. Speaking of which, don't forget to add `<script src="src/index.js" charset="utf-8"></script>` to the head of your `index.html`

This may sound silly but step 1 should be:

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  alert("LOADED");
});
```

Until you see the alert, don't move forward. What would be some reasons you might not see the alert?

Now let's fetch the notes (remember that the route our real backend would be 'http://localhost:3000/api/v1/notes' whereas here we'll make the request to our json-server non-namespaced routes )

### SUPER IMPORTANT

If you're using the `json-server` package for your server, your endpoint should be `http://localhost:3000/notes` and `http://localhost:3000/api/v1/notes` if you're using the server we built in part one.

Let's save ourselves trouble in the future and add this to the top of our `index.js`

`const endPoint = <YOUR_ENDPOINT>`

---

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const endPoint = "http://localhost:3000/api/v1/notes";
  fetch(endPoint)
    .then(res => res.json())
    .then(json => console.log(json));
});
```

If you see the notes printed to the console, you're good to move forward.

The next step is getting the Notes added to the DOM. No problem, add an empty div or ul element to index.html and go ahead and add each note title, along with an edit button.

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const endPoint = "http://localhost:3000/api/v1/notes";
  fetch(endPoint)
    .then(res => res.json())
    .then(json =>
      json.forEach(note => {
        const markup = `
        <li>
          <h3>${note.title}
            <button>edit</button>
          </h3>
        </li>`;

        document.querySelector("#notes-list").innerHTML += markup;
      })
    );
});
```

There are many ways to do this. The above snippet is not super pretty, but it works.

### Step 2: Refactor

If our only deliverable was to show text on the page, our code would be sufficient. There's a real deficiency with our current implementation though.

Think about the next step where a user clicks one of the edit buttons. Given our current implementation how could we a) determine which note got clicked on and b) show more information about that note (the content of the note)?

Please take a moment to think this through and make sure you understand the following before moving forward.

---

The only way to solve this problem would be to grab the text of the h3 element from the DOM, use that title to query our backend and do something in our Rails controller along the lines of...

```ruby
@note = Note.find_by(title: params[:title])
```

This should feel really annoying. We _just_ had access to this data when we retrieved all the notes, but we effectively threw it away.

This is where we can refactor to use Object Orientation. We can take advantage of the encapsulation provided by objects to store _all_ the data about a particular note in one place.

A second annoyance we might notice about our current implementation is that when the edit button is clicked, nothing on the button itself indicates what note the button is for. We have to look at the text of it's parent h3 element. Let's solve this in our refactor as well.

Refactored code:

```javascript
/* create a file src/note.js and link to it from your index.html */
class Note {
  constructor(data) {
    this.id = data.id;
    this.title = data.title;
    this.content = data.content;
    Note.all.push(this);
  }

  renderListItem() {
    return `
    <li>
      <h3>${this.title}
        <button data-id=${this.id}>edit</button>
      </h3>
    </li>`;
  }
}

Note.all = [];
```

Note: if you are not familiar with html5 data-attributes [check them out](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes).
We totalllyyyy could have taken the id of the note and added it to the DOM in the button's id or class properties. However, html ids and classes are typically used for css, not to store data.

But this is exactly what data-attributes are for and should make our lives easier. The important takeaway here is that the data our application logic depends on **lives in the DOM itself and we must put it there somehow.** _Understanding that is more important than how exactly we put that data there._

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const endPoint = "http://localhost:3000/api/v1/notes";
  fetch(endPoint)
    .then(res => res.json())
    .then(json => {
      json.forEach(note => {
        const newNote = new Note(note);
        document.querySelector(
          "#notes-list"
        ).innerHTML += newNote.renderListItem();
      });
    });
});
```

### Step 3: Clicking the 'edit' button & showing a form

Our code above was a true refactoring: we didn't change any functionality, we only changed (and hopefully improved) the implementation details.

Now let's add the ability to click an edit button and show a filled out form. As always, when dealing with handling events we'll want to break this down into a couple steps.

1 - Can we respond to the event at all? First let's just `console.log` or `alert` something on a click.

2 - Can we then `console.log` some data specific to the event. We'll try to `console.log` the whole note object we're trying to edit.

3 - Only after all that is wired up will we attempt to show a form with the correct values.

The first step, though straightforward, involves some decision making--where should the code that attaches the event listener go?

There is no right answer here. An argument could be made that it is the responsibility of the Note class, something like `Note.addEditListeners()`. The choice we will go with is to make a class called `App` that will be responsible for higher level things like attaching event listeners.

```javascript
/* src/app.js */
class App {
  attachEventListeners() {
    document.querySelector("#notes-list").addEventListener("click", e => {
      console.log("clicked");
    });
  }
}
```

_Note: we won't go into [event delegation](https://learn.jquery.com/events/event-delegation/) in detail here, but because the edit buttons are dynamically added to the page we cannot put the event listeners on them directly. We have to put the listener on a static element, i.e. the parent ul, and delegate the listening down to the children_

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const app = new App();
  app.attachEventListeners();

  /*your previous code below...*/
});
```

When the page loads we'll create an instance of our App and call the `attachEventListeners` function. If you see 'clicked' in the console move on to the next step.

---

You are very much encouraged to try to get the next step working on your own. You need to a) grab the data-id of the clicked button out of the DOM and b) find the associated note instance. Try it on your own. Below is an implementation that works.

```javascript
/* src/app.js */
class App {
  attachEventListeners() {
    document.querySelector("#notes-list").addEventListener("click", e => {
      const id = parseInt(e.target.dataset.id);
      const note = Note.findById(id);
      console.log(note);
    });
  }
}
```

```javascript
/* src/note.js */
class Note {
  // ... prev code

  static findById(id) {
    return this.all.find(note => note.id === id);
  }
}
```

Once we have the note instance the next step is pretty easy. Just as we can call a prototype method `note.renderListItem` on a note instance we'll make a prototype method `note.renderUpdateForm` and attach HTML to the DOM. This is like telling a note object: 'use your state (all the information about this note) to create an update form for it'.

An implementation:

```javascript
/* src/note.js */
class Note {
  // ... prev code

  renderUpdateForm() {
    return `
    <form data-id=${this.id}>
      <label>Title</label>
      <p>
        <input type="text" value="${this.title}" />
      </p>
      <label>Content</label>
      <p>
        <textarea>${this.content}</textarea>
      </p>
      <button type='submit'>Save Note</button>
    </form>
  `;
  }
}
```

```javascript
/* src/app.js */
class App {
  attachEventListeners() {
    document.querySelector("#notes-list").addEventListener("click", e => {
      const id = parseInt(e.target.dataset.id);
      const note = Note.findById(id);
      document.querySelector("#update").innerHTML = note.renderUpdateForm();
    });
  }
}
```

### Step 4: Making the PATCH request

When the form is submitted we need to make a PATCH request to our server to update this note record in our database. Like before, we will start with a straightforward approach and refactor.

It seems like we already have a place in our app where we attach event listeners. Let's add our code there. I will skip a few steps here and go straight to the implementation. When you are trying to grab data from the DOM in your own projects try things like

`const title = e.target.querySelector('input').value;`

Open up the console, use a debugger, and play around!

```javascript
class App {
  attachEventListeners() {
    // prev code...
    document.querySelector("#update").addEventListener("submit", e => {
      e.preventDefault();
      const id = parseInt(e.target.dataset.id);
      const note = Note.findById(id);
      const title = e.target.querySelector("input").value;
      const content = e.target.querySelector("textarea").value;

      const bodyJSON = { title, content };
      fetch(`http://localhost:3000/api/v1/notes/${note.id}`, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json"
        },
        body: JSON.stringify(bodyJSON)
      })
        .then(res => res.json())
        // our backend responds with the updated note instance represented as JSON
        .then(updatedNote => console.log(updatedNote));
    });
  }
}
```

This should update the note in the database on submit. We haven't updated anything on the DOM with our response, but if we refresh the page we should see the updated note. Remember that when we refresh the page it makes a fresh fetch request to the API for data and renders those notes on the page. Before tackling the next step, let's refactor!

### Step 5: Refactor (Adapter Pattern)

One thing that might become hard to follow about the current code is that we are making our fetch requests in all different places. We get all the notes on document ready in `index.js`, and we patch to update the note in `app.js`. You can imagine as our application grows we will be making more AJAX requests scattered across more places.

Let's create a class whose only responsibility is to communicate with the API. We can call this our adapter class or API class.

We're aiming to have code that can be used like this:

```javascript
app.adapter.fetchNotes().then(json => {
  json.forEach(note => {
    document.querySelector("#notes-list").innerHTML += new Note(
      note
    ).renderListItem();
  });
});
```

What then is the return value of `app.adapter.fetchNotes()`? Something that we can call `.then` on, so it must be a promise! All of our adapter methods should return promises that we can then chain `.then` onto and manipulate the data as needed. This is cool because the adapter can hide away some of the implementation details of `fetch`, such as setting the headers, converting the response into json, [error handling our fetch requests](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch), etc.

```javascript
/* src/adapter.js */
class Adapter {
  constructor() {
    this.baseUrl = "http://localhost:3000/api/v1";
  }
  fetchNotes() {
    return fetch(`${this.baseUrl}/notes`).then(res => res.json());
  }

  updateNote(id, body) {
    return fetch(`${this.baseUrl}/notes/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json"
      },
      body: JSON.stringify(body)
    }).then(res => res.json());
  }
}
```

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const app = new App();
  app.attachEventListeners();

  app.adapter.fetchNotes().then(json => {
    json.forEach(note => {
      document.querySelector("#notes-list").innerHTML += new Note(
        note
      ).renderListItem();
    });
  });
});
```

```javascript
/* src/app.js */
class App {
  constructor() {
    this.adapter = new Adapter();
  }

  attachEventListeners() {
    //... prev code
    document.querySelector("#update").addEventListener("submit", e => {
      e.preventDefault();
      const id = parseInt(e.target.dataset.id);
      const note = Note.findById(id);
      const title = e.target.querySelector("input").value;
      const content = e.target.querySelector("textarea").value;
      const jsonBody = { title, content };
      this.adapter
        .updateNote(note.id, jsonBody)
        .then(updatedNote => console.log(updatedNote));
    });
  }
}
```

Now everything should work as before, but we do have some methods that are long and hard to follow. The following is an example of some continued refactoring, abstracting away and encapsulating methods for re-use:

```javascript
/* src/index.js */
document.addEventListener("DOMContentLoaded", () => {
  const app = new App();
  app.attachEventListeners();
  app.adapter.fetchNotes().then(app.createNotes);
});
```

```javascript
class Adapter {
  constructor() {
    this.baseUrl = "http://localhost:3000/api/v1/";
    this.headers = {
      "Content-Type": "application/json",
      Accept: "application/json"
    };
  }

  fetchNotes() {
    return this.get(`${this.baseUrl}/notes`);
  }

  updateNote(id, body) {
    return this.patch(`${this.baseUrl}/notes/${id}`, body);
  }

  get(url) {
    return fetch(url).then(res => res.json());
  }

  patch(url, body) {
    return fetch(url, {
      method: "PATCH",
      headers: this.headers,
      body: JSON.stringify(body)
    }).then(res => res.json());
  }
}
```

```javascript
/* src/app.js */
class App {
  constructor() {
    this.adapter = new Adapter();

    this.handleEditClick = this.handleEditClick.bind(this);
    this.handleFormSubmit = this.handleFormSubmit.bind(this);
    this.createNotes = this.createNotes.bind(this);
    this.addNotes = this.addNotes.bind(this);
  }

  attachEventListeners() {
    document
      .querySelector("#notes-list")
      .addEventListener("click", this.handleEditClick);
    document
      .querySelector("#update")
      .addEventListener("submit", this.handleFormSubmit);
  }

  // notice the previous functionality is broken up
  // into two different methods for future re-use...
  createNotes(notes) {
    notes.forEach(note => {
      new Note(note);
    });
    this.addNotes();
  }

  addNotes() {
    document.querySelector("#notes-list").innerHTML = "";
    Note.all.forEach(
      note =>
        (document.querySelector(
          "#notes-list"
        ).innerHTML += note.renderListItem())
    );
  }

  handleFormSubmit(e) {
    e.preventDefault();
    const id = parseInt(e.target.dataset.id);
    const note = Note.findById(id);
    const title = e.target.querySelector("input").value;
    const content = e.target.querySelector("textarea").value;

    const bodyJSON = { title, content };
    this.adapter
      .updateNote(note.id, bodyJSON)
      .then(updatedNote => console.log(updatedNote));
  }

  handleEditClick(e) {
    const id = parseInt(e.target.dataset.id);
    const note = Note.findById(id);
    document.querySelector("#update").innerHTML = note.renderUpdateForm();
  }
}
```

_Note: we need to be extra sure that every time we call one of the methods on an app instance such as handleEditClick, that the context (i.e. what 'this' is) is always the app instance we'd expect. Sometimes when we pass around functions as callbacks the context can get mixed up. There are several ways to solve this, but notice in the constructor we have code like `this.handleFormSubmit = this.handleFormSubmit.bind(this);`. With this setup, we can bind the correct context once and then not have to worry about it ever again :)_

### Step 6: The Home Stretch, Updating the Note

We now have a really solid codebase to work with to solve the last problem, updating the DOM after changing a note title.

Here's what we need to do:

1 - Locate the code that logs the response from the PATCH request.

2 - Take the response JSON and update the note in our client side collection of all the notes (`Note.all`).

3 - Re-render all the notes.

We'll choose to re-render all of the notes instead of just re-rendering one. We already have a method that can display all notes, lets's use it!

The code only needs to change in a few places:

```javascript
/* src/app.js */
class App {
  // ...

  addNotes() {
    document.querySelector("#notes-list").innerHTML = "";
    Note.all.forEach(
      note =>
        (document.querySelector(
          "#notes-list"
        ).innerHTML += note.renderListItem())
    );
  }

  handleFormSubmit(e) {
    // ...
    this.adapter.updateNote(note.id, bodyJSON).then(updatedNote => {
      const note = Note.findById(updatedNote.id);
      note.update(updatedNote);
      this.addNotes();
    });
  }
}
```

```javascript
/* src/note.js */
class Note {
  // ...
  update({ title, content }) {
    this.title = title;
    this.content = content;
  }
}
```

Note: If you are not familiar with what is going on in the line `update({ title, content })`, look into [ES6 Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)\_

Go out there and build something really cool!
