# Serverless applications

Serverless APIs are the cool new thang everyone is talking about. They don't use a traditional web server to send requests, instead you write **functions** which the platform executes for you.

Today we're using Vercel as that platform, only because it's the easiest to use, but there are many out there that do this.

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

## Next

Once you're done with that, you can move on to [**Vercel Develop**](./08%20-%20Vercel%20Develop.md). 👏👏👏
