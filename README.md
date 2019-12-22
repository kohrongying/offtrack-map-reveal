# Offtrack Map
---
For the forgetful. View it [here.](https://kohrongying.github.io/offtrack-map-reveal)

### Running
```
npm run dev
```
Navigate to [localhost:5000](http://localhost:5000).

### Building
```
npm run build
```

You can run the newly built app with `npm run start`.

Built with [Svelte](https://svelte.dev/)

### Concluding Thoughts

I thought that this would be a perfect opportunity to practice my css grid, since the destinations fit perfectly in a 3x3 grid.

Only halfway through building it, I  wanted a drag and drop feature. [Dragula](https://github.com/bevacqua/dragula) was a really useful and simple package for this. I could not get it to work at first, but the magic was to use the `onMount` method in Svelte.

The last hurdle was changing the orientation from portrait to landscape, as I wanted to utilise the longer width in landscape mode. In the end, I just forcefully rotated the entire element when the media query was portrait. Thank goodness for CSS media queries! 

It was a quick and fun project :)