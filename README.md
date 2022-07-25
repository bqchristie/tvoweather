# tvoweather

This is a simple vue app that uses the open weather API to finds cities and display their current weather.  I tried to 
use as few dependencies as possible for the sake of simplicity.  In "real-life" there would be some tools I'd reach for
that would keep things more concise.

There are already a number of things to refactor but I had to timebox this.  Hopefully there is enough here to warrant a discussion.

Thanks for reviewing!


The app can be viewed at:

**https://amazing-pika-d3b75f.netlify.app/**

### TODO

1. Accessibility review (particularly with city selector.)
2. Sass loader - refactor styles into better system
3. Debounce the city search
4. Add .env to put API key


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
