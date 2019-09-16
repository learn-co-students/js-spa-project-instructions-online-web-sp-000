# Planning, Tips, and Support

Unlike labs, projects do not have the guardrails of a README and tests to structure your work. Instead, you need to design and plan your features yourself. These tips should help you plan your project effectively.

## Project Support

As you complete the labs leading up to the project, you should receive an email from the Section Coach about scheduling a Project Planning Meeting. 

In your Project Planning Meeting, you and your coach will discuss topics like
- The basic story of your application
- The core features you are planning to build
- Your data models and their relationships
- Challenges you expect to face

If you do not receive an email, and have checked your spam folder, please reach out to the Section Coach by slack or email. If reaching out by slack, please include your email address in your message.

After the Project Planning Meeting, there are a few ways for you to find support with your project as you build:

- Attend weekly Study Groups and Project Office Hours listed on the Study Groups tab in your learn.co account. See upcoming study groups at [Learn.co Study Groups](https://learn.co/study-groups). Office Hours and topic-specific Study Groups are the best place to leverage technical support from your instructors and fellow students!
- Participate in the slack channel to discuss project concepts and questions.
- Lastly, if you have questions about the project or the assessment that cannot be addressed using the above options, please do not use the Ask A Question feature. Instead, please reach out to your Section Coach and/or Section Lead. You can find your Section Lead using the following link: [Who are the section leads](http://help.learn.co/instructional-support/receiving-course-support/who-are-the-section-leads).

Reminder: your work should be your own! For more details about the rules of the road, see [Project Rules of the Road](/project-rules-of-the-road.md).

## Communicating About Your Project

Your project should shine. To help your vision come across clearly, you should create and submit the following along with your project:

- A `README.md` file with a description of the application
- A _Blog Post_ telling the story behind the application, challenges you overcame, and what you learned
- A 2-4 minute _Video Demo_ walking through the features of your application

The audience should be the technically-minded outsider that stumbles on your project. Do not expect that they will have any of the context behind your project - you need to tell them that context. Focus on telling them the story of your application. What problem does it solve? How do the features come together to solve that problem? Why did you decide to build it?

## Guidelines for working solo

When working on a team, you get to have explicit conversations about the design of your application and the choices you make. Since you are working solo, you need to be extra clear - and **write down** - the decisions you make about your project. This will not only help you think more clearly about your design, it will also give context to instructors if you ask for help, and help you communicate about your project when you are done.

## Planning

- Brainstorm the problem that you want your application to solve. It's okay to take inspiration for the features you want to build from other sites or projects you've seen (it's not okay to use their code though).
- Plan what features your app will have. You can write **User Stories** to help make it clear what you are planning to build.
- Model your domain. You need to know what the nouns in your project are - the objects in the 'world' of your application. It can be helpful to draw the relationships between your models.
- Plan how your features will work.

## User Stories

**User Stories** are a powerful tool for succinctly describing a set of functionality in terms of what it allows the user to do.

Some examples of user stories:

- A user is able to view travel locations, add new travel locations, edit travel locations and delete travel locations.
- A user is able to review a travel location
- A user is able to edit and delete a review for a travel location

When you have written the user stories for your application, you can turn them into the technical requirements needed in order to turn those stories into working features.

## Domain Modeling

As you turn your user stories into more clear technical specifications for features, you can start by modeling the data that your application will store and show. Identify the nouns in your stories, their properties, and their relationships.

A description of the domain for the above stories might be:

- A travel location has a name, a description, a location and an image URL
- A review has a comment and a rating
- A review belongs to one travel location (a travel location has many reviews)
- A travel location has an average rating that is calculated by its aggregate review ratings

Later on, you will be ready to create the database schema and application models corresponding to this domain.

## MVP ASAP

(Minimum Viable Product, As Soon As Possible)

Building things is hard. It's very hard to predict what will be difficult in a project. Sometimes things that appear on the surface to be easy will end up taking hours of debugging.

With that in mind, it's important to build a Minimum Viable Product (MVP) as quickly as possible. Instead of getting stuck on advanced features, start with a basic working version of the application, then steadily add features piece by piece.

## Build vertically, not horizontally

It's easy to end up having to do lots of rework and fixing depending on how you order the things you build in your application, particularly if you build 'horizontally'.

You can visualize all the parts you of an app you need to build as a grid, with the features along the x axis (columns) and the different layers of the stack along the y axis.

|                    | View Location | Browse Locations | Edit Location | Add Review | Edit Review |
| ------------------ | ------------- | ---------------- | ------------- | ---------- | ----------- |
| Styling            |               |                  |               |            |             |
| View Logic         |               |                  |               |            |             |
| Data Fetching      |               |                  |               |            |             |
| Controller actions |               |                  |               |            |             |
| Seed Data          |               |                  |               |            |             |
| Models             |               |                  |               |            |             |
| Migrations         |               |                  |               |            |             |

A strong temptation is build your project row-by-row. It _feels_ easy to start by writing all the migrations for all your models, then all the models, etc.

**Do not do this!**

If you try to build all your migrations, then all your models, then all your controllers, then all your fetch calls, then all your view logic, you will have a bad time. Inevitably, your view logic will end up requiring changes to the underlying layers, and you will write code that doesn't get used.

Instead, build **vertically**, column-by-column. Write code for all the vertical layers involved in one feature before moving on to the next feature. That way, you'll minimize rewriting, and end up with working features without waste.

The project process should look like:

- Planning: Write down your ideas (use diagrams!)
- Start by creating the frontend and backend directories
- Build the **R** from CRUD for just one model, _vertically!_ That means one migration, one model, one controller action, one `fetch` call, and one DOM update. Add seed data and confirm that your code works by testing it visually.
- Then, build the next CRUD action (maybe Create or Update), again building **vertically**.
- Continue building features one by one, (_vertically!_)
- Add feature by feature, not model by model or layer by layer.
- Test each feature, add styles, and create seed data as you go (not all at once at the end)
