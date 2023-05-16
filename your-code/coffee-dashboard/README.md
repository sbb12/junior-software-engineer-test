This is a copy of the root readme

# Amberside Energy Code Test

This is my solution to the junior software engineer technical test


# Architecture and Design choices

I chose [SvelteKit](https://kit.svelte.dev/) as my framework to develop this solution.
SvelteKit is an excellent choice for small applications like this as it is highly performant, easy to use and very versatile.

For similar reasons, [TailwindCss](https://tailwindcss.com/) powers the styling as it allows for rapid design which suits smaller projects well. 
Though, the styling is minimal only to show functionality.

# Omissions  

From the time i was expected to spend on this, i decided to omit some features which i thought would be beyond the scope of this test. These are:
- More sophisticated error handling if the api fails
- Validation of received data
- Loading state during fetch, which helps with UX
- Components which help with organisation when the project grows.
- Responsive design or pagination.

# Build

From my understanding of the problem presented, The api endpoints are expected to be on a separate server so that will need to be launched separately. This is done from the ```/server``` folder.

```bash
$ cd server
$ npm install
$ npm run dev
```

To build and launch my solution

1. Enter the ```/coffee-dashboard``` folder and install the dependencies

```bash
$ cd your-code
$ cd coffee-dashboard
$ npm install
```

2. Run the server
```bash
$ npm run dev -- --open
```

This should launch the server and open a browser on the homepage where the dashboard is located.
