# Deploy Angular App to Netlify

In netlify, create a new site and link to your target github repostiory. There are three inputs in deploy settings:

- Branch to deploy: usually it's the master branch
- Build command: it is `ng build --prod` to build the production version of your Angular application.
- Publish directory: usually it is `dist/your-project-name`.

The following is the screen shot for the `angular-tour-of-heroes` project.

![Deploy to netlify example](./deploy-to-netlify.png)

Then click `Deploy Site`, your Angular Application should be up and running in one or two minutes.
