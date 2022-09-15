# MVC Lecture

This is an MVC lecture I created for my coworkers. Feel free to view the slides (including the flow charts) and download the source code:
https://slides.com/robinherzig/mvc/
https://github.com/100devs/todo-mvc

## What is MCV?

"MVC (Model-View-Controller) is a pattern in software design commonly used to implement user interfaces, data, and controlling logic. It emphasizes a separation between the software's business logic and display. This "separation of concerns" provides for a better division of labor and improved maintenance. Some other design patterns are based on MVC, such as MVVM (Model-View-Viewmodel), MVP (Model-View-Presenter), and MVW (Model-View-Whatever)." -MDN Web Docs Glossary

But what does that mean?

MVC is an architectural paradigm that creates a structure where applications can be created modularly.

This separation of backend processes helps the application scale in complexity because it's easier to code, debug, and test each part as an individual.

It's composed of three main components:
1. Model: Accesses the database.
2. View: Renders HTML for the client.
3. Controller: Processes CRUD requests and server-side logic.

## What do these components do?

The model defines data structure and sends or receives this data from the database.

The view handles data presentation, typically populated using information retrieved from the model and then rendered using a template engine.

The controller receives every request and, using information provided within it, is coded to direct that information to the correct receiver.

## How do these components interact?

1. The Client makes a GET request.
2. The Controller requests data from the Model.
3. The Model requests data from the Database.
4. The Database responds with the data.
5. The Model responds to the Controller.
6. The Controller requests a render from the View.
7. The View responds with a render.
8. The Controller responds to Client with a rendered webpage.

What patterns do we see?
- The Client only connects to the Controller.
- The Controller connects between the Model and View.
- The Model connects to the Database.

Again, this separation of backend processes helps the application scale in complexity because it's easier to code, debug, and test each part as an individual.

## How can this architecture be improved?

We can amplify the advantages of MVC by adding another component: the Routes.

To create the route file, separate the route from the controller:
```router.get('/', async (req,res)=>{
  try{  const todoItems = await Todo.find()
    const itemsLeft = await Todo.countDocuments({completed: false})
    res.render('todos.ejs', {todos: todoItems, left: itemsLeft})
  }catch(err){
   console.log(err)
  }
})
```

The route: `router.get('/',`
The controller: Everything else.

An addition, installing the Mongoose module through NPM makes writing MongoDB logic much easier.

"Mongoose provides a straight-forward, schema-based solution to model your application data. It includes built-in type casting, validation, query building, business logic hooks and more, out of the box." -Mongoose JS official website

The Mongoose module includes features to create schemas, which are object templates for MongoDB documents.

These templates can be used to limit data types:
```const blogSchema = new Schema({
  title:  String, // String is shorthand for {type: String}
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});
```

## What is a practical example?

Here is the workflow for a GET request, including the file and the code.

Go to routes/todos.js to find the route
server.js
```const todoRoutes = require('./routes/todos')
app.use('/todos', todoRoutes)
```

Go to controllers/todos.js to complete the route
routes/todos.js
```const todosController = require('../controllers/todos')
router.get('/', todosController.getTodos)
```

Go to models/Todo.js to request data from the database
controllers/todos.js
```const Todo = require('../models/Todo')
getTodos: async (req,res)=>{
  try{
    const todoItems = await Todo.find()
    const itemsLeft = await Todo.countDocuments({completed: false})
    res.render('todos.ejs', {todos: todoItems, left: itemsLeft})
  }catch(err){
    console.log(err)
  }
}
```

Use the mongoose module to return the documents
models/Todos.js
```const TodoSchema = new mongoose.Schema({
  todo: {
    type: String,
    required: true,
  },
  completed: {
    type: Boolean,
    required: true,
  }
})
module.exports = mongoose.model('Todo', TodoSchema)
```

After retrieving information from the database, respond to the controller
models/Todos.js
```const TodoSchema = new mongoose.Schema({ 
  todo: {
    type: String,
    required: true,
  },
  completed: {
    type: Boolean,
    required: true,
  }
})
module.exports = mongoose.model('Todo', TodoSchema)
```

Render todos.ejs
controllers/todos.js
```const Todo = require('../models/Todo')
getTodos: async (req,res)=>{
  try{
    const todoItems = await Todo.find()
    const itemsLeft = await Todo.countDocuments({completed: false})
    res.render('todos.ejs', {todos: todoItems, left: itemsLeft})
  }catch(err){
    console.log(err)
  }
}
```

Respond to the client with the rendered HTML
controllers/todos.js
```const Todo = require('../models/Todo')
getTodos: async (req,res)=>{
  try{
    const todoItems = await Todo.find()
    const itemsLeft = await Todo.countDocuments({completed: false})
    res.render('todos.ejs', {todos: todoItems, left: itemsLeft})
  }catch(err){
    console.log(err)
  }
}
```

## How are these files organized?

Each page may have its own set of Routes, Controllers, Models, and Views.

The website as a whole shares the same server.js file and database.

The overall file structure may look like this:
```config
    database.js

controllers
    homepage.js
    about.js

models
    homepage.js
    about.js

public
    ...

routes
    homepage.js
    about.js

views
    homepage.js
    todos.ejs

server.js
```

Again, let's work through a GET request so we can recognize the file structure better:
1. The Client makes a GET request.
2. The server.js receives the request, forwards it to the applicable route.
3. The Routes sends the request to the correct controller.
4. The Controller requests data from the Model.
5. The Model requests data from the Database.
6. The Database responds with the data.
7. The Model responds to the Controller.
8. The Controller requests a render from the View.
9. The View responds with a render.
10. The Controller responds to Client with a rendered webpage.

## Conclusion

Hopefully you found this guide helpful to better understand MVC architecture.

Remember, the slides (including the flow charts) and source code are both available:
https://slides.com/robinherzig/mvc/
https://github.com/100devs/todo-mvc

In addition, feel free to message me if you have any questions or comments.

Thank you!
