# sot-full-stack-javascript
Summer of Tech full stack JavaScript Masterclass

# Introduction

Welcome 🖐️🖐️🖐️

In this JavaScript Masterclass we'll be looking at the full development lifecycle of a website - Back end and front end, from creation to production deployment.

The goal of the session will be to give you a simple boilerplate portfolio website.

Topics we'll cover:

* Write a small Node API
* Write a simple front end with React
* Deploy our website to the web using Heroku

## Prerequisites / Install

Don't worry about cloning this repo - we're making everything from scratch today.

* A basic understanding of JavaScript
* [Node.js](https://nodejs.org/) installed
* An editor of your choice ([VS Code](https://code.visualstudio.com/) is great)
* Git
* [nodemon](https://github.com/remy/nodemon) installed (`npm i -g nodemon`)
* [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) installed
* A Heroku account (free)
* [Logged into your heroku cli](https://devcenter.heroku.com/articles/heroku-cli#getting-started)

# What does "full stack" mean?
"Full stack" is one of those weird terms that means different things to different people/companies.

Generally "full stack" developers are knowledgeable across all of the layers of development.

[This good article](https://medium.com/coderbyte/a-guide-to-becoming-a-full-stack-developer-in-2017-5c3c08a1600c) describes a modern full stack web developer as knowing:
* HTML/CSS
* JavaScript
* Back-End Language
* Databases & Web Storage
* HTTP & REST
* Web Application Architecture
* Git
* Basic Algorithms & Data Structures

Sounds like a lot.. But if we break it down, it's a little less scary. We'll try do that today!

# Project setup

Before we kick into writing code, let's start with one of the most important layers - our **Web Application Architecture**

* Node backend with an [Express](https://expressjs.com/) web server
* [React](https://reactjs.org/) front end

Start by creating a new folder for our project:
```
mkdir my-portfolio && cd my-portfolio
```
Now, let's generate the `package.json` file (default values are fine):
```
npm init
```

Great! We should have a folder with a single `package.json` file in it!

This will be our `root` directory, for future reference.

# Web server
Now that we've got a home for our code, let's look at our **Back-End Language**.

We're going to use [Express](https://expressjs.com/) to create our web server.

Start by installing it using npm:
```
npm i express
```

Then, create a new file called `index.js` in the root directory.

In `index.js` we'll include the code to kick our web server into life.

```
const express = require('express');
const app = express();
const path = require('path');

const port = process.env.PORT || 5000;

app.listen(port);
console.log(`My Portfolio is listening on ${port}`);
```

Start'er up with:
```
node index.js
```
If all goes well, we should see `My Portfolio is listening on 5000` printed to the console.

## API

Cool, now let's put that web server to use and get some data flowing though it by creating a small API.

We'll eventually pull this data from github, but for now, let's return some simple data from an endpoint.

In `index.js`
``` 
let projectsData = [
    {
        name: 'Trade Me',
        html_url: 'http://preview.trademe.co.nz',
        description: 'I helped people buy stuff'
    }
];
```
Change the data to reflect a project you've worked on, if it doesn't have a url, just use http://google.com as a placeholder.

Now, let's return that data when we visit `/api/projects`
```
app.get('/api/projects', (req, res) => {
    // Return projects as json
    res.json(projectsData);
});
```
This basic api, touches our **HTTP & REST** layer of development.

See if it worked by restarting your server and head to http://localhost:5000/api/projects

You should be greeted by the data from your `projectsData` object!

# Deploying

Now for the fun part. Making this accessible to everyone outside your local network.

There are thousands of ways this could be done such as using our own VPN and setting up our own server, but often that costs a lot of money and time. 

In recent years, cloud companies like Amazon (AWS), Google Cloud and Heroku have developed products that streamline this process for us.

We simply provide them with a git repo that runs a web server - they host it and handle all the messy stuff like load, when our app goes viral.

## Heroku
Heroku isn't often used by enterprise companies, but it's great for small personal projects because it's **FREE** and **EASY AF**.

If you haven't already - create a free account of your own at https://heroku.com

And download the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli), then log in using:
```
heroku login
```

The basic idea of Heroku is that it can take a git repo, and does all the magic of setting it up on line for you.

So let's create that git repo!
```
git init
```

Heroku works by looking inside our `package.json` file for a `"start"` command under `"scripts"`.

We can add one:
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js",
  },
```

Commit our amazing work:

```
echo node_modules > .gitignore
git add .
git commit -m "added web server and simple api"
```
Then we're ready to create our Heroku app:
```
heroku create
```

Now, deploy!

```
git push heroku master
```

It should spit out a bunch of messages (hopefully no errors), followed by a url. Head to that url and let's see if our api works.

`<your url>/api/projects`

If you see the object you created before, well done! You just created an api.

# Front end
Every 6 seconds someone comes out with a new JavaScript front end framework. Fact. (not really)

React is a good option because its heavily used and supported. People have created great tooling to make it easy to rapidly develop applications using it.

One of those tools is [create-react-app](https://github.com/facebook/create-react-app) which spins up a boilerplate react app which handles all the bits and bobs (like productionizing our code) which are sometimes annoying to do.

Install it:
```
npm install -g create-react-app
```

Then, in your root directory of your project:
```
create-react-app client
```

This will spin up our front end within a folder called `client`.

cd into `client` and start the application:
```
cd client
npm start
```

Then open http://localhost:3000 to see what we've just created.

## Proxy
So we don't have to worry about ports (3000/5000), Create React App will proxy API requests from the React app to the Express app if we add a “proxy” key in package.json like this:

```
"proxy": "http://localhost:5000"
```

This goes in `client/package.json`, *not* in the Express app’s package.json, and it will be ignored by Heroku after deploying.

Now, replace `src/App.js` with:

```
import React, { Component } from 'react';
import './App.css';

class App extends Component {
    // Initialize state
    state = { projects: [] };

    // On load
    componentDidMount() {
        this.getProjects();
    }

    // Set state with our projects
    getProjects = () => {
        fetch('/api/projects')
            .then(res => res.json())
            .then(projects => this.setState({projects}));
    }

    render() {
        const { projects } = this.state;

        return (
            <div className="App">

                <h1>Hi, my name is <your name here></h1>
                <h3>I'm a developer</h3>

                <h4>Here are a few of my projects</h4>

                {
                    projects.length ? (
                        projects.map((project, index) => (
                            <div key={project.name}>
                                <p><b><a href={project.html_url}>{project.name}</a></b></p>
                                <p>{project.description}</p>
                            </div>
                        ))
                    ) : (
                        <div>
                            I don't have any projects
                        </div>
                    )
                }
            </div>
        );
    }
}

export default App;
```

If you don't quite understand what's going on, don't worry! A few [React tutorials](https://reactjs.org/tutorial/tutorial.html) will be all you need.

Essentially, we're using `fetch` to access our api we created earlier, grab the project data, then looping over our projects and displaying them.

Start up **both** your back end and front end:

Root directory terminal window, then in a second terminal window in `client`:
```
npm start
```

If you head over to http://localhost:5000 we should see a very rough website starting to form!

# Production
The code we have is great! But it's designed to be run in Development mode on our local dev machine. 

* Errors very specific, so we can fix them easily
* Source maps are included so we can debug live in the browser (with code we wrote) 

However, these features aren't ideal to ship to users of our websites. We want to ship **fast** code that's been **bundled** ready to be consumed in the most efficient manner by a browser. 

## Build
If we look inside `client/package.json` we can see the scripts that we can run. 

```
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  },
```

We're interested in the "build" script. It does all the productionizing for us.

It creates a full production build in a folder `client/build` - that's what we want our Express application to serve to our users.

## Serving our React from Express

Back in our `index.js`, we need to tell Express to serve the static files that are generated in `client/build`.

```
// Serve static files from the React app
app.use(express.static(path.join(__dirname, 'client/build')));

...

// The "catchall" handler: for any request that doesn't
// match one above, send back React's index.html file.
app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname+'/client/build/index.html'));
});
```

## Deploy our prod build
In our root `package.json` file, let's tell heroku to run a production build when we deploy:

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js",
    "heroku-postbuild": "cd client && npm install && npm run build"
  },
```

`"heroku-postbuild"` will be run by Heroku after we push a new build.

We can commit and deploy our new code:

```
git add .
git commit -m "added front end"
git push heroku master
```

You may notice this time it takes a little longer to build, as Heroku is running the production build process.

# Add some live data
It would be nice if we could add in some live data from github.

The github api is really easy to use, some endpoints don't even require auth 😍

The endpoint we're interested in today is to list all of our projects.

`https://api.github.com/users/<your github user name>/repos`

E.g. https://api.github.com/users/marcusklein/repos returns all my projects and a bit of data about them.

In `index.js` with our express app, we can get the data we need from github using `node-fetch` (fetch is browser only).

```
npm i node-fetch
```

Then, we can:

```
const fetch = require('node-fetch');

...

let projectsData = [
    {
        name: 'Trade Me',
        html_url: 'http://preview.trademe.co.nz',
        description: 'An online marketplace, where kiwis buy and sell'
    }
];

const githubRepoUrl = 'https://api.github.com/users/marcusklein/repos';

function getProjects () {
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

            projectsData = projectsData.concat(gitHubProjects);
            console.log(`Loaded ${projectsData.length} projects`);
        });
}

getProjects();
```

The nice part about this is it's caching the data on the server, and stripping out all the data from github that we don't need.

Full stack developers should have a relatively good idea what makes a website fast and performant. 

Note: This means the data that comes through to our server will only be updated each time we restart it. That's called *"busting the cache"*.

Sanity check that it works (never deploy broken code) by `npm start`'ing both processes.

No errors? Great! Shipit!

```
git add .
git commit -m "added live data from github"
git push heroku master
```

Wait for it to build, then we'll see how it looks 👍️

Kinda average, right? But that's for you to change!

# Where to next?

Employers LOVE seeing the work you've done. Showing them that you have a rough idea of the process of shipping code goes a long way in an interview. Feel free to use this demo as a rough boilerplate in creating a portfolio you're proud of.

Things to do:
* Style it! Basic HTML and CSS goes a long way. Stackoverflow is your friend.
* [Change the URL](https://devcenter.heroku.com/articles/custom-domains). Github student pack gives you a free .me domain name! Heroku lets you use it!
* Add your contact details. Some more data? Pictures of your work!
* [Analytics](https://analytics.google.com/analytics/web/) - see who's been checking you out!



Thanks 🙌 🙌 🙌 🙌

Hit me up if you have any issues/questions!
