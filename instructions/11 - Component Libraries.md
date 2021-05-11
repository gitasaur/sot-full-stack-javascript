# Component Libraries

Styling websites isn't easy, but thankfully we don't always have to start from scratch. Using **Component Libraries** we can easilly make our website look beautiful with only a few lines of code.

We'll be using the [Material-UI](https://material-ui.com/) for this. It's one of the best out there, also one of the most used.

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

Component libaries are great. Have a play with Material-UI's components and style up your website.

## Next

Once you're done with that, you can move on to [**Deploy Live**](./12%20-%20Deploy%20Live.md). 👏👏👏
