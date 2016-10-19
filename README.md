# PHPantastic Packages

This is a slide deck developed for ZendCon 2016. It was developed using
reveal.js.

## Running the slides

The slide deck uses the markdown features of reveal.js, so it requires that you
run the deck using npm.

First, install dependencies.

```bash
$ sudo npm install -g grunt # Necessary to fire up the server
$ yarn install              # Preferred, as there's a lockfile
$ npm install               # Also works
```

Once installed:

```bash
$ npm start
```

This will fire up the server, and open the slides in your preferred browser.

## Notes

There are slide notes available. Press "s" in the browser to spawn the presenter
view.

Additionally, the full narrative is available in the `markdown/outline.md` file,
which was the source material for the `markdown/slides.md` file.

## TODO

There are a few placeholders in the `slides.md` for images. I've gathered these
in the `img/` folder. The talk can be given with or without them.

## License

[CC BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/)
