![ambiènce](https://user-images.githubusercontent.com/28996247/129722737-0a4c7f65-834c-4261-8dd9-3c993d8ebd99.png)
<hr />

### [Ambiènce](https://ambience.srg.id.au/) is a study dashboard that has a to-do list, built in media player and modern design.
It is like other study dashboards like [Studyboard](https://studyboard.srg.id.au) (also by me) and [LifeAt.io](https://lifeat.io) except that it is super fast and lightweight.

For example, the entire built website is 1.8 megabytes, **1 megabyte of which is the open graph image**, which is never accessed by the browser anyways. If you ignore all of the image resources, the built code with all dependencies comes out to 232KB. 

This makes it super fast to load and get started with.

```bash
# As of 2021-08-17

$ ls
Octicons-mark-github.svg	build				global.css
android-chrome-192x192.png	favicon-16x16.png		index.html
android-chrome-512x512.png	favicon-32x32.png		og.png
apple-touch-icon.png		favicon.ico			site.webmanifest

$ du -sh .
1.8M	.

$ du -h *.png
 68K	android-chrome-192x192.png
408K	android-chrome-512x512.png
 64K	apple-touch-icon.png
4.0K	favicon-16x16.png
4.0K	favicon-32x32.png
1.0M	og.png

$ du -sh build/
232K	build/
```

## Technologies
This site is built using [Svelte](https://svelte.dev/). A full list of dependences can be found in `package.json`.
