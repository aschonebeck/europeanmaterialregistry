# European Material Registry

An open atlas of where physical things are made in Europe — quarries, foundries,
mills, kilns and workshops, indexed by place, material and process.

Speculative project. The places and material traditions are real; some of the
company names are invented. Photography is used to illustrate the concept and is
not owned by the project.

## Running it locally

Open `index.html` in a browser. Nothing to install and no build step.

Everything is in one file on purpose. Browsers block a page loaded over `file://`
from fetching sibling files, so a version split across several scripts would only
work behind a web server — inlining them means double-clicking the file works.

Two things come from a CDN and so need a connection: React and Babel, and the
country outlines the map is drawn from. The rest — every photograph, all the
data, the whole interface — is local.

## Putting it online

Any static host will serve it as-is. On GitHub Pages:

1. Upload the contents of this folder so `index.html` sits at the repository root.
2. Settings → Pages → Deploy from a branch, `main` and `/ (root)`.

## What is where

- `index.html` — the entire interface, with the dataset near the top
- `styles.css` — base styles, the map, and the mobile breakpoint
- `images/` — material and process photographs

## The data

`window.REGISTRY` near the top of `index.html` holds one object per maker. Order
matters: entry 1 fills the tall preview tile on the Index, and entries 2 and 3
stack beside it, so those three read as one composition.

Each entry can name two photographs. `image` is the material itself and appears
in the gallery; `image2` shows the process and appears in the detail view. Both
are optional — anything without them falls back to a shared image for that
material, which is why neighbouring entries in the list should not share one.

## The map

Country outlines come from Natural Earth via `world-atlas`, drawn as vectors
rather than raster tiles so the labels can be set in the same typeface as the
rest of the site.

Pin positions are real coordinates in `window.COORDS`. Label positions are not
automatic: a few countries are placed by hand in the `anchors` object inside the
map component, because a polygon centroid falls in the wrong place for Norway,
where thousands of vertices along the fjords drag the average toward Sweden.
Labels are ranked by country size and hidden when they would collide, so the
largest names survive when central Europe runs out of room.
