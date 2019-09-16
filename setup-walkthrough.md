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
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
   Sample text - replace me! 
  </body>
</html>
```

Open this file in your browser to make sure things are working.

To get the JavaScript part of the project up and running, link the JavaScript file to your HTML page with a `<script>` tag:

```html
<script type="application/javascript" src="src/index.js" charset="UTF-8"></script>
```

To check that your JavaScript file is linked correctly, add a log statement, refresh, and view the result in the JavaScript console.

```js
console.log("testing...")
```
(in `src/index.js`)

## Connecting Rails and JavaScript - Make a Request with `fetch`

Now that you have a Rails server running and have an HTML page with some JavaScript executing, you need to make sure that they can talk to each other.

If you haven't set up a controller yet on the Rails side, you can use this test code to make sure that things are working correctly. If you've set up a controller, feel free to test with that instead.


In `walkthrough-backend/app/controllers/application_controller.rb`:

```ruby
class ApplicationController < ActionContoller::API
  def test
    render json: { test: "success" }
  end
end
```

In `walkthrough-backend/config/routes.rb`:

```ruby
Rails.application.routes.draw do
  get '/test', to: 'application#test'
end
```

In `walkthrough-frontend/src/index.js`:

```javascript
// test that we can get data from the backend
const BACKEND_URL = 'localhost:3000';
fetch(`${BACKEND_URL}/test`)
  .then(response => response.json())
  .then(parsedResponse => console.log(parsedResponse));
```

If you have your Rails server running, and you refresh your html page, you should see the success message logged to the JavaScript console.

## Building the rest of your application

Okay, so if you've gotten this far, you have a Rails app running and you have a vanilla HTML and JavaScript client app that can talk to the Rails app. You'll want to add features to your project, to match your design. Where to next?

Some things to consider:

- To add _nouns_ to your world, create models in rails
- To _show_ your nouns, you'll need:

  1. A controller action to send the data
  2. A fetch request to ask for the data 
  3. Some JavaScript code to handle DOM-rendering 

- To make your page respond to the user, you'll need _event listeners_
- To keep your code clean, you should use JavaScript _classes_
- To organize your code, you can use multiple JavaScript files (don't forget to add `<script>` tags for each one!)

Good luck, and happy building!
