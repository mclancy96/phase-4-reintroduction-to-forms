
# Reintroduction to Forms: From HTML & React to Rails

Welcome to your next step in mastering web development! In this lesson, we'll revisit the concept of forms—one of the most fundamental ways users interact with web applications. You already know how to build forms in HTML and React, and you've just started exploring Rails. This lesson will connect those dots, deepen your understanding, and prepare you for Rails-specific form helpers in the next lessons.

---

## Forms You Already Know

Let's start by recapping what you already know about forms in HTML and React.

### Forms in HTML

HTML forms are containers for input elements like text fields, checkboxes, radio buttons, and submit buttons. When a user fills out a form and submits it, the browser sends the data to a specified URL using a specified HTTP method (usually GET or POST).

```html
<form action="/posts" method="POST">
  <label for="post_title">Post title:</label><br>
  <input type="text" id="post_title" name="post[title]" /><br>

  <label for="post_description">Post description:</label><br>
  <textarea id="post_description" name="post[description]"></textarea><br>

  <input type="submit" value="Submit Post" />
</form>
```

**Key attributes:**

- `action`: Where the form data is sent.
- `method`: How the data is sent (GET or POST).
- `name`: How the data is structured when sent to the server.

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

**Key concepts:**

- State variables manage input values.
- The `onSubmit` handler controls what happens when the form is submitted.
- You can validate, transform, or send data anywhere you need.

---

## How Forms Work: The Lifecycle

Let's zoom out and look at the lifecycle of a form submission:

1. **User fills out the form in the browser.**
2. **Browser packages up the data from each input.**
3. **Browser sends the data to the server via HTTP (GET or POST).**
4. **Server receives the data, usually as a hash or object.**
5. **Server processes the data (e.g., saves to a database, sends a response, or displays an error).**

For example, submitting the above HTML form would result in:

```ruby
params = {
  "post" => {
    "title" => "My post title",
    "description" => "My post description"
  }
}
```

---

## A Preview of Rails Forms

Now let's see how Rails fits into this process. We'll look at how Rails receives form data, how routes and controllers work, and introduce a key security concept.

### How Rails Receives Form Data

When you submit a form in Rails, the data arrives in the `params` hash. This hash is available in your controller actions and lets you access the values entered by the user.

```ruby
# /app/controllers/posts_controller.rb
def create
  puts params.inspect # See all incoming data
  # ...existing code...
end
```

### Routing and Controller Actions

To process form submissions in Rails, you need to set up routes and controller actions. For example:

```ruby
# /config/routes.rb
resources :posts, only: [:index, :new, :create]
```

This creates:

- `GET /posts` (index)
- `GET /posts/new` (new form)
- `POST /posts` (create)

Your controller needs an action to handle the form data:

```ruby
# /app/controllers/posts_controller.rb
def create
  # Access form data from params
  Post.create(title: params[:post][:title], description: params[:post][:description])
  redirect_to posts_path
end
```

**Note:** At this stage, we're assigning each attribute individually, not using mass assignment. This is for security and clarity, and we'll discuss why in a future lesson on Strong Params.

### Manual ERB Form Example

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

### Security Preview: CSRF and Authenticity Tokens

Web applications must protect against malicious requests. One common attack is Cross-Site Request Forgery (CSRF), where a hacker tricks a user into submitting a form without their knowledge. **The good news:** Rails automatically protects you by requiring a unique authenticity token with each form submission. If you use Rails form helpers (which you'll learn about soon), this is handled for you!

If you ever build a form by hand, you may need to include the token yourself:

```erb
<!-- /app/views/posts/new.html.erb -->
<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>" />
```

But don't worry—Rails is designed to keep you safe by default. You'll learn more about this in future lessons.

### Why Rails Helpers Matter

If you've ever built forms by hand—whether in HTML, React, or even in Rails views—you know it can get tedious fast. Every input, label, and hidden field must be written out, and it's easy to make mistakes or forget something important (like security tokens or proper naming conventions). As your applications grow, maintaining and updating forms can become a real headache.

This is where Rails truly shines. Rails provides powerful form helpers that let you generate forms dynamically, safely, and with much less code. These helpers handle the repetitive details for you, ensure your forms are secure, and make your code easier to read and maintain. In the next few lessons, you'll see how Rails form helpers can transform the way you build forms—making the process faster, safer, and much more enjoyable.

---

## Reflection & Practice

1. **Explain in your own words:** What happens when a user submits a form in a web application?
2. **Code Exercise:** Write an HTML form for creating a new comment with fields for `author` and `body`. Label each input clearly.
3. **React Practice:** How would you manage the state of a form with multiple fields in React?
4. **Rails Routing:** What routes do you need in Rails to support displaying a form and processing its submission?
5. **Security Reflection:** Why is CSRF protection important, and how does Rails implement it?

## Additional Resources

- [MDN Web Docs: HTML Forms](https://developer.mozilla.org/en-US/docs/Learn/Forms)
- [React Docs: Forms](https://react.dev/learn/forms)
- [Rails Guides: Action Controller Overview](https://guides.rubyonrails.org/action_controller_overview.html)
- [Rails Guides: Routing](https://guides.rubyonrails.org/routing.html)
- [Rails Guides: Security](https://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf)
