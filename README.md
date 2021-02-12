# Technical Documentation Versioning

An exploration of versioning schemes for technical documentation using Hugo and Netlify.

This presentation was built for the [Cloud Native Computing Foundation](https://www.cncf.io/).

# Build

To build the html, first preview the presentation:

```shell
marp -p slides/documentation-versioning.md
```

Then copy the `documentation-versioning.html` file that was created in the `slides` folder to `index.html` and edit any `../assets` reference to `./assets`.

_Note_: this is done because the presentation format is rendered slightly differently with the preview.

To build the pdf and pptx:

```shell
marp --allow-local-files slides/documentation-versioning.md --pdf --output dist/documentation-versioning.pdf

marp --allow-local-files slides/documentation-versioning.md --pptx --output dist/documentation-versioning.pptx
```

# Meta

This presentation is built using [Marp](https://marp.app/) and is published at https://technical-documentation-versioning.netlify.app.

The research used to build this presentation can be found at [this project's wiki](https://github.com/nate-double-u/technical-documentation-versioning/wiki)

## License

This work is licensed under a [CC-BY-4.0 license](./LICENSE). 

