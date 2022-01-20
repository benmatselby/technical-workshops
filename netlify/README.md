# Netlify

**Time allocation:** 45 minutes

This workshop aims to provide an overview of Netlify. We will get you up and running with a simple web application deploying into Netlify. We will cover things like the CLI and Functions. If you're wanting to ditch managing in infrastructure for your web applications, then this is the workshop for you.

We have a rough agenda of:

- Ben: What is Netlify?
  - [Website](https://www.netlify.com)
  - [Tech Radar](https://radar.emisgroup.uk/content/technology/netlify)
- Ben: What the benefits are.
  - [White paper that Team Heisenberg created](https://radar.emisgroup.uk/white-papers/netlify.pdf)
- Ben: The dashboard
- Ben: The CLI
- Ben: Logs
- Eleanor: The configuration
- Eleanor: The deployments
  - Deploy previews.
  - Branch based deployments.
  - Publish a deployment.
  - Back out a deployment.
- Q&A

## Notes

### What is Netlify?

Our Tech Radar states

> Netlify is a specialist JAMStack web hosting provider, focussing on value add features for Engineering and Product teams. The have emerged by designing services on top of traditional cloud providers like AWS to accelerate product development by abstracting away infrastructure and providing a smoother developer experience.

JAMStack is JavaScript, APIs, and Markup.

- It's a platform as a service, building on the same building blocks of AWS as we have been building on.
- Their business is infrastructure for JAMStacks. Our business is providing Healthcare software to our customers and users.

### What are the benefits

- Suggest reading the [white paper](https://radar.emisgroup.uk/white-papers/netlify.pdf) that Dario and Team Heisenberg wrote up.
- It's a dedicated web hosting platform.
- Alongside Vercel, these are becoming the defacto ways to quickly deliver web applications.
- Features constantly being added, which we pay for.

### Dashboard

- Log in and overview
- Sites
  - Create a site using hello-netlify
  - Deployments which Eleanor will cover off
  - Plugins, think along the lines of GitHub Actions, there is a marketplace, we use some already for the Lighthouse stuff.
    - We built our own too, for Percy.
  - Functions - AWS Lambdas, without all the drama, just drop a file into your codebase. (Demo)
  - Analytics.
  - Domain management - written up in the Tech Radar.
    - We can hook into the URLs we already have, and get Netlify to setup Lets Encrypt certificates for us.
