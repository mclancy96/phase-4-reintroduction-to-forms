# Reintroduction to Forms: From HTML & React to Rails

Welcome to your next step in mastering web development! In this lesson, we'll revisit the concept of forms—one of the most fundamental ways users interact with web applications. You already know how to build forms in HTML and React, and you've just started exploring Rails. This lesson will connect those dots, deepen your understanding, and prepare you for Rails-specific form helpers in the next lessons.

## What Are Forms?

Forms are the primary way users send data to a web application. Whether you're signing up for a newsletter, searching for a product, or posting a comment, you're using a form. Forms allow users to input information, which is then sent to the server for processing.

### Forms in HTML

Let's start by recalling how forms work in plain HTML. An HTML form is a container for input elements like text fields, checkboxes, radio buttons, and submit buttons. When a user fills out a form and submits it, the browser sends the data to a specified URL using a specified HTTP method (usually GET or POST).

```html
<form action="/posts" method="POST">
 <label for="post_title">Post title:</label><br>
 <input type="text" id="post_title" name="post[title]" /><br>

 <label for="post_description">Post description:</label><br>
 <textarea id="post_description" name="post[description]"></textarea><br>

 <input type="submit" value="Submit Post" />
</form>
```

**Key points:**

- The `action` attribute tells the browser where to send the form data.
- The `method` attribute specifies how to send the data (GET or POST).
- Each input's `name` attribute determines how the data is structured when sent to the server.

### Forms in React

In React, forms are built using JSX and managed with state. You control the values of inputs using state variables and handle submission with event handlers.

```jsx
// /src/components/PostForm.jsx
import React, { useState } from 'react';

function PostForm() {
 const [title, setTitle] = useState('');
 const [description, setDescription] = useState('');

 const handleSubmit = (e) => {
  e.preventDefault();
  // Send data to server or update state
  console.log({ title, description });
 };

 return (
  <form onSubmit={handleSubmit}>
   <label>Post title:</label><br />
   <input
    type="text"
    value={title}
    onChange={e => setTitle(e.target.value)}
    name="post[title]"
   /><br />

   <label>Post description:</label><br />
   <textarea
    value={description}
    onChange={e => setDescription(e.target.value)}
    name="post[description]"
   /><br />

   <button type="submit">Submit Post</button>
  </form>
 );
}

export default PostForm;
```

**Key points:**

- React forms use state to manage input values.
- The `onSubmit` handler lets you control what happens when the form is submitted.
- You can validate, transform, or send data anywhere you need.

## How Forms Work in Web Applications

When a form is submitted, the browser packages up the data from each input and sends it to the server. The server receives this data (often as a hash or object), processes it, and responds accordingly—maybe by saving it to a database, sending back a confirmation, or displaying an error.

In Rails, this data arrives in the `params` hash. For example, submitting the above HTML form would result in:

```ruby
params = {
  "post" => {
    "title" => "My post title",
    "description" => "My post description"
  }
}
```

## Routing and Controller Actions for Forms in Rails

To process form submissions in Rails, you need to set up routes and controller actions. Let's walk through the process step by step.

### 1. Setting Up the Route

You need a route that responds to the form submission. In Rails, this is usually a `POST` route for creating new records.

```ruby
# /config/routes.rb
resources :posts, only: [:index, :new, :create]
```

This creates:

- `GET /posts` (index)
- `GET /posts/new` (new form)
- `POST /posts` (create)

### 2. Creating the Controller Action

Your controller needs an action to handle the form data. For example:

```ruby
# /app/controllers/posts_controller.rb
def create
 # Access form data from params
 Post.create(title: params[:post][:title], description: params[:post][:description])
 redirect_to posts_path
end
```

**Note:** At this stage, we're assigning each attribute individually, not using mass assignment. This is for security and clarity, and we'll discuss why in a future lesson on Strong Params.

### 3. Rendering the Form View

You need a view file to display the form:

```erb
<!-- /app/views/posts/new.html.erb -->
<h3>Post Form</h3>
<form action="<%= posts_path %>" method="POST">
 <label>Post title:</label><br>
 <input type="text" id="post_title" name="post[title]" /><br>

 <label>Post description:</label><br>
 <textarea id="post_description" name="post[description]"></textarea><br>

 <input type="submit" value="Submit Post" />
</form>
```

## Understanding Params in Rails

When you submit a form, Rails collects the data and stores it in the `params` hash. This hash is available in your controller actions and lets you access the values entered by the user.

```ruby
# /app/controllers/posts_controller.rb
def create
 puts params.inspect # See all incoming data
 # ...existing code...
end
```

## HTTP Methods and Form Actions

HTML forms can use different HTTP methods:

- **GET**: Used for retrieving data (e.g., search forms).
- **POST**: Used for sending data to the server (e.g., creating a new post).

In Rails, most forms that create or update data use POST. The method attribute in your form tag tells the browser which HTTP verb to use.

## Security: CSRF and Authenticity Tokens

Web applications must protect against malicious requests. One common attack is Cross-Site Request Forgery (CSRF), where a hacker tricks a user into submitting a form without their knowledge. Rails protects you by requiring a unique authenticity token with each form submission.

If you see an error like `ActionController::InvalidAuthenticityToken`, it means your form is missing this token. Rails form helpers add it automatically, but if you're building forms by hand, you need to include it:

```erb
<!-- /app/views/posts/new.html.erb -->
<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>" />
```

This ensures only legitimate requests are processed by your application.

## Transition: From Manual Forms to Rails Helpers

If you've ever built forms by hand—whether in HTML, React, or even in Rails views—you know it can get tedious fast. Every input, label, and hidden field must be written out, and it's easy to make mistakes or forget something important (like security tokens or proper naming conventions). As your applications grow, maintaining and updating forms can become a real headache.

This is where Rails truly shines. Rails provides powerful form helpers that let you generate forms dynamically, safely, and with much less code. These helpers handle the repetitive details for you, ensure your forms are secure, and make your code easier to read and maintain. In the next few lessons, you'll see how Rails form helpers can transform the way you build forms—making the process faster, safer, and much more enjoyable.

---

## Reflection Questions & Exercises

1. **Explain in your own words:** What happens when a user submits a form in a web application?
2. **Code Exercise:** Write an HTML form for creating a new comment with fields for `author` and `body`. Label each input clearly.
3. **Rails Routing:** What routes do you need in Rails to support displaying a form and processing its submission?
4. **React Practice:** How would you manage the state of a form with multiple fields in React?
5. **Security Reflection:** Why is CSRF protection important, and how does Rails implement it?

## Additional Resources

- [MDN Web Docs: HTML Forms](https://developer.mozilla.org/en-US/docs/Learn/Forms)
- [React Docs: Forms](https://react.dev/learn/forms)
- [Rails Guides: Action Controller Overview](https://guides.rubyonrails.org/action_controller_overview.html)
- [Rails Guides: Routing](https://guides.rubyonrails.org/routing.html)
- [Rails Guides: Security](https://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf)

---

In the next lesson, you'll learn how Rails form helpers make building forms easier, safer, and more powerful. For now, make sure you understand the basics of forms in HTML, React, and Rails, and practice building and submitting forms in each environment!
