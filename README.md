# European Material Registry

An open atlas of where physical things are made in Europe — quarries, foundries,
mills, kilns and workshops, indexed by place, material and process.

Speculative project. The places and material traditions are real; some of the
company names are invented. Photography illustrates the concept and is not owned
by the project.


## Running it

Open `index.html` in a browser. No build step, nothing to install.

Everything is in one file on purpose: browsers block a page loaded over `file://`
from fetching sibling scripts, so a version split across several files would only
work behind a web server. Double-clicking this one works.

Two things load from the internet, so the first run needs a connection: React
and Babel, and the country outlines the map draws from. Every photograph and all
the data are local.


## Putting it online

Any static host serves it as-is. On GitHub Pages: upload the contents of this
folder so `index.html` is at the repository root, then Settings → Pages → Deploy
from a branch, `main` and `/ (root)`.


## Where to change things

**Colours** → `styles.css`, the TOKENS block at the top. Six values carry the
whole interface. The JavaScript reads the same tokens, so editing one there
changes both the stylesheet and the inline styles.

**Layout and spacing** → `styles.css` is organised in four numbered sections:
tokens, base, map, mobile. Anything that only happens on a phone is in section 4.

**Text and structure** → `index.html`. It is one file, but divided into seven
labelled blocks, each marked with a banner naming what it covers:

    THE DATA         every maker in the registry
    MAP              Leaflet setup, pins, country labels
    SHARED           palette, logo, navigation, the Index, gallery tiles
    HOMEPAGE         the place list, and the detail view
    GEOGRAPHY        places grouped by country
    SUBMIT           the contribution form
    APP              which screen shows, and the state they share

Search for `source:` to jump between them.


## Adding a maker

Add an object to `window.REGISTRY` in the first block. Order matters: entry 1
fills the tall preview tile on the Index and entries 2 and 3 stack beside it, so
those three read as one composition.

    {
      id: "SE-001",
      name: "Luleå Järnverk",
      place: "Luleå",
      country: "SE",              // two letters; must exist in COUNTRY_NAMES
      region: "Norrbotten",
      material: "steel",          // steel ceramic glass wool wood stone paper
      process: "forged",
      scale: "industrial",        // artisan | small-batch | industrial
      founded: 1941,
      certs: ["ResponsibleSteel"],
      blurb: "One sentence on what they make and how.",
      image:  "images/steel-pillow.jpg",   // the material — shown in the gallery
      image2: "images/steel-plate.jpg",    // the process — shown in the detail view
    }

Both images are optional. Without them an entry falls back to a shared image for
its material, so neighbours in the list should not both rely on the fallback or
they will show the same photograph.

For the maker to appear on the map, add its town to `window.COORDS` with real
latitude and longitude.


## The map

Country outlines come from Natural Earth via `world-atlas`, drawn as vectors
rather than raster tiles — which is what lets the labels be set in the same
typeface as the rest of the site.

Two things are deliberately not automatic. A handful of countries are positioned
by hand in `anchors`, because a polygon centroid lands in the wrong place for
Norway, where thousands of vertices along the fjords drag the average toward
Sweden. And labels are ranked by country size and hidden when they would
collide, so the large orienting names survive when central Europe runs out of
room.
