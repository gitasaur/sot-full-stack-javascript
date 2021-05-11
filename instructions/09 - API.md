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

Start (or restart) the dev server, run:
```
vercel dev
```

After the project has built, head to [http://localhost:3000/api/projects](http://localhost:3000/api/projects) to see the response from the api we just created.

## Next

Once you're done with that, you can move on to [**Front End**](./10%20-%20Front%20End.md). 👏👏👏
