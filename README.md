# Sapper in the front, Gatsby in the back
Sapper is really cool for building static pages but Gatsby has a fantastic ecosystem of interesting and useful plugins, making
it super easy to mesh content from different sources.
What if both could be used together?

## A word of caution
My intent here was to confirm that something like this would be feasible - not that it's a good idea.
Expect rough edges.

## Installing it
Installation is less than ideal at the moment and goes along these lines:

```bash
cd backend
npm i
cd ..
cd frontend
npm i
```

This will first install Gatsby related stuff in the `backend` directory and will then continue with installing Sapper
within the `frontend` directory.

## Running it
To run it you first need to run the backend to make sure that Gatsbys GraphQL server is up and running before starting the Sapper frontend.
There's probably ways to deal with that in one terminal but I'll do two anyway.

### Running the backend
You'll need `gatsby-cli` to be installed globally for this to work.

```bash
cd backend
gatsby develop
```

### Running the frontend
It's important that the Gatsby GraphQL server is up an running so do this in another terminal:

```bash
cd frontend
npm run dev
```

## Production build
Start the Gatsby backend as described above, then export the Sapper app:

```bash
cd frontend
npm run export
```

Once the build is done you can find your full static site within `frontend/__sapper__/export`, ready to be deployed somewhere.

## Where is the content?
Right now I only played around with Markdown and the `gatsby-source-filesystem` plugin. The blog posts can be found within the `pages` directory.
It should be possible to install any of the Gatsby source plugins to add more GraphQL queries that can be loaded from Sapper.

## What's missing?

- Better development experience
- MDX equivalent, maybe
- Sanity check...