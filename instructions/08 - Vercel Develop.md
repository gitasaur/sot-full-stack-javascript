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

## Next

Once you're done with that, you can move on to [**API**](./09%20-%20API.md). 👏👏👏