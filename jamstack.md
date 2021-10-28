# Jamstack

## Reference

<https://blog.openreplay.com/jamstack-a-new-way-to-think-about-web-development-build-and-delivery>

- Developers write code, commit and push it to a source repository application like GitHub.
- The integration with a CDN provider(like Netlify or Vercel) kicks off a workflow that starts the build to create prebuilt content.
- The prebuilt content then gets deployed to a CDN by the CDN provider, and a unique secured URL gets generated for you. The URL gets generated for every commit so that you can preview the app in a production environment before deploying them.
- You share this URL with your end-users or customers to access the app.
- Users request the resources from the CDN(available in proximity) that is serving the prebuilt content. For any services, the API call goes to separate microservices hosted on the cloud.