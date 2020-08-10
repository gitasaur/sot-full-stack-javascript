# JavaScript Web Application Masterclass
Summer of Tech JavaScript Web Application Masterclass

# Introduction

Welcome 🖐️🖐️🖐️

In this JavaScript Masterclass we'll be looking at the full development lifecycle of a website - Back end and front end, from creation to production deployment.

The goal of the session will be to give you a simple boilerplate portfolio website.

Topics we'll cover:

* Write a small **serverless** Node API
* Write a simple front end with React
* Deploy our website to the web using Vercel

## Prerequisites / Install

Don't worry about cloning this repo - we're making everything from scratch today.

* A basic understanding of JavaScript
* [Node.js](https://nodejs.org/) installed
* An editor of your choice ([VS Code](https://code.visualstudio.com/) is great)
* [Vercel CLI](https://vercel.com/download) installed
* A Vercel account (free)
* Logged into your Vercel cli
    * `$ vercel login`

# What does Web Application development in 2020 look like?
There is a lot to consider when building a web app, not to mention learn, and it can seem daunting knowing where to start.

* What framework should I use? React? Angular? Vue?
* How will I serve data? REST? GraphQL? Databases?
* Where will I host the app?
* How do I deploy the app?

A good reference is [https://roadmap.sh/](https://roadmap.sh/).

It gives good pathways for both [frontend](https://roadmap.sh/frontend) and [backend](https://roadmap.sh/backend), as well as the best resources to learn them.

As you can see, there is a lot that goes into modern web applications, but today we'll learn the basics from both to get you up and running so you can venture down the paths with a bit more context.

# Tooling

Thankfully, as developers we don't need to know everything to write a web application as tooling exists to abstract away complexity so we can dive straight into to writing our application.

A good example is [Create React App](https://create-react-app.dev/). It sets us up with boilerplate code, build scripts to optimise perfomance and a nice development environment.

Then we can combine this with [Vercel](https://vercel.com) as our tool to handle the deployment of our site. It deploys our app to a webserver without needing to write one 🤯

# Project setup

Before we kick into writing code, let's explore our **Web Application Architecture**

* Node backend with a **serverless** web server
* [React](https://reactjs.org/) front end

We'll use `create-react-app` to generate the boilerplate of our project. This will give us everything we need to get cracking straight away.

```
npx create-react-app my-portfolio
```

Change directory into the folder that was just created:

```
cd my-portfolio
```

Looking in this folder, we have a functioning website!

The `package.json` file is located in the `root` of our folder and the files we edit are located in `src`.

Spin it up with:
```
npm start
```

Wait for it to build, then head over to [http://localhost:3000](http://localhost:3000) and you should see your website!

# Deploying

Vercel makes it incredibly easy to deploy our `create-react-app` project. It identifies that it's a React project, then does all the build and deploy steps for us.

Make sure you're logged in:
```
vercel login
```

Then deploy! (make sure you're in the project `root`)
```
vercel
```

Easy! This takes a moment, but will give you a link to the Vercel site to check the progress and visit the site.

# Serverless applications

Serverless APIs are the cool new thang everyone is talking about. They don't use a traditional web server to send requests, instead you write **functions** which the platform executes for you.

Today we're using ZEIT as that platform, only because it's the easiest to use, but there are many out there that do this.

* [AWS Lambda](https://aws.amazon.com/lambda/)
* [Google Cloud Functions](https://cloud.google.com/functions/docs/)
* [Azure Functions](https://azure.microsoft.com/en-us/services/functions/)

## Advantages of Serverless

1. Easy to deploy 
    * Just write and deploy your functions!
2. Low cost
    * No server to maintain, only runs when it's called
3. Better Scalability
    * Platform handles all the load for you

Traditional Node API:
```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

Serverless API:
```
module.exports = (req, res) => {
    res.send('hello world);
}
```

Everything is just a **function**, which is run by the platform when it's needed.


# Vercel Develop
Our application is alive and deployed, but what about local development, so we don't have to deploy every time we want to make a change.

`vercel` comes with a handy dev mode, but to use that we first must add a script in our `package.json` file:
```
"dev": "react-scripts start --port $PORT",
```
e.g.
```
...
"scripts": {
    "dev": "react-scripts start --port $PORT",
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
...
```
Then when we run `vercel dev`, it will first spin up our "serverless" environment (api), then the React app. All changes will be instantly refreshed!

# API
Let's make our API. `vercel` expects all API's to come from a folder called `api`. In the root directory, create a new folder called that.

Then, in this folder, we'll create our JavaScript file which holds our **function**.

**The name of this file is important.** It's the route that we'll access our REST endpoint from. e.g. `projects.js` will be accessed from `http://my-vercel-url/api/projects`.

Create a file called `projects.js` inside the `api` folder.

Inside this file, put:
```
module.exports = (req, res) => {
    let projectsData = [
        {
            name: 'BNZ',
            html_url: 'http://bnz.co.nz',
            description: `Banks aren't boring`
        }
    ];

    res.json(projectsData);    
}
```
Feel free to change the `name`, `html_url`, and `description` parts yourself to something personal, if you'd like.

This is just dummy data - we'll fetch our real data soon.

If you're not already running the dev server, run:
```
now dev
```

After the project has built, head to [http://localhost:3000/api/projects](http://localhost:3000/api/projects) to see the response from the api we just created.

## Add some real data
It would be nice if we could add in some live data about our projects from github.

The github api is really easy to use, some endpoints don't even require auth 😍

The endpoint we're interested in today is to list all of our projects.

`https://api.github.com/users/<your github user name>/repos`

E.g. https://api.github.com/users/marcusklein/repos returns all my projects and a bit of data about them.

In `index.js` with our express app, we can get the data we need from github using `node-fetch` (fetch is browser only).

```
npm i node-fetch
```

Then back in our `projects.js` file, replace the contents with:
```
const fetch = require('node-fetch');

const githubRepoUrl = 'https://api.github.com/users/marcusklein/repos';

module.exports = (req, res) => {
    fetch(githubRepoUrl)
        .then(res => res.json())
        .then(projects => {

            const gitHubProjects = projects.map(project => {
                return {
                    name: project.name,
                    html_url: project.html_url,
                    description: project.description
                }
            });

            res.json(gitHubProjects);    
        });
}
```

Save, wait for the app to rebuild, then you should see your projects returned!

That's all we need to do for our API.

# Front End
We've got some data from our API, now let's display it on our React webpage.

Head into the `src` folder, from your `root` directory. This is where the heart of your React website lives.

The `App.js` file is the one controling the page we can see on our website, so this is the one that we'll be editing.

Now, replace `src/App.js` with:


<div key={project.name}>
                                <p><b><a href={project.html_url}>{project.name}</a></b></p>
                                <p>{project.description}</p>
                            </div>

```
import React, { useState, useEffect } from 'react';
import './App.css';
import Button from '@material-ui/core/Button';


const App = () => {
    // Initialize state
    const [ projects, setProjects ] = useState([]);

    // Get projects
    useEffect(() => {
      fetch('/api/projects')
            .then(res => res.json())
            .then(projects => setProjects(projects));
    },[]);

    return (
        <div className="App">

            <h1>Hi, my name is [YOUR NAME]</h1>
            <h3>I'm a developer</h3>

            <h4>Here are a few of my projects</h4>

            {
                projects.length ? (
                    projects.map((project) => (
                        <div style={{padding: 10}} key={project.name}>
                            <p><b><a href={project.html_url}>{project.name}</a></b></p>
                            <p>{project.description}</p>
                        </div>
                    ))
                ) : (
                    <div>
                        Loading projects..
                    </div>
                )
            }
        </div>
    );
}

export default App;
```

If you don't quite understand what's going on, don't worry! A few [React tutorials](https://reactjs.org/tutorial/tutorial.html) will be all you need.

Essentially, we're using `fetch` to access our api we created earlier, grab the project data, then looping over our projects and displaying them.

### Check out your site
Run your dev server again, and check out your site!
You should see all your projects list from your API.

## Component Libraries

Styling websites isn't easy, but thankfully we don't always have to start from scratch. Using **Component Libraries** we can easilly make our website look beautiful with only a few lines of code.

We'll be using the [Material-Ui](https://material-ui.com/) for this. It's one of the best out there, also one of the most used.

Install it:
```
npm install @material-ui/core
```

Load the default Roboto font in the `public/index.html` file. Put it above the `<title>`.

```
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />

```

Then back in your `src/App.js`

We'll import the Button component:
```
import Button from '@material-ui/core/Button';
```

Then replace our links (inside the `projects.map()` function) with the Button components:
```
<div style={{padding: 10}} key={project.name}>
    <Button 
        variant="contained"
        href={project.html_url}>
        {project.name}
    </Button>
</div>
```

Now with a refresh we should see better buttons.

Component libaries are great. Have a play with Material-iu's components and style up your website.

# Deploy live again
Let's publish our final version!

```
vercel
```

After it's built, we should see our app live, using our own API we've built.

# Where to next?

Employers LOVE seeing the work you've done. Showing them that you have a rough idea of the process of shipping code goes a long way in an interview. Feel free to use this demo as a rough boilerplate in creating a portfolio you're proud of.


# Thanks 🙌 🙌 🙌 🙌

Hit me up if you have any issues/questions!

* [marcusklein@me.com](mailto:marcusklein@me.com)
* Twitter: [@nzmilky](https://twitter.com/nzmilky)
* LinkedIn: [https://www.linkedin.com/in/marcus-klein-35683b93/](https://www.linkedin.com/in/marcus-klein-35683b93/)
