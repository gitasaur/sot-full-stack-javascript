# Front End
We've got some data from our API, now let's display it on our React webpage.

Head into the `src` folder, from your `root` directory. This is where the heart of your React website lives.

The `App.js` file is the one controling the page we can see on our website, so this is the one that we'll be editing.

Now, replace `src/App.js` with:


```
import React, { useState, useEffect } from 'react';
import './App.css';


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

## Add some real data
It would be nice if we could add in some live data about our projects from github.

The github api is really easy to use, some endpoints don't even require auth 😍

The endpoint we're interested in today is to list all of our projects.

`https://api.github.com/users/<your github user name>/repos`

E.g. https://api.github.com/users/marcusklein/repos returns all Marcus' projects and a bit of data about them.

Just like with our react code above we can use `fetch` to get data, but sadly it doesn't come out of the box with node.js (as it's not running on a browser). So we can install `node-fetch` to enable this.

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

### Check out your site
Run your dev server again, and check out your site!
You should see all your projects list from your API.

## Next

Once you're done with that, you can move on to [**Component Libraries**](./11%20-%20Component%20Libraries.md). 👏👏👏
