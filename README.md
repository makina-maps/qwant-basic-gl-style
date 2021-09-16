# qwant-basic-gl-style

[![CI status](https://github.com/Qwant/qwant-basic-gl-style/workflows/qwant-basic-gl-style%20build/badge.svg)](https://github.com/Qwant/fafnir)

This is the Mapbox GL basemap style used for [Qwant Maps](https://github.com/Qwant/QwantMaps).

![preview](https://qwant.github.io/qwant-basic-gl-style/preview/custom.png)

This style uses a vector tile schema adapted from [OpenMapTiles](https://github.com/Qwant/OpenMapTiles) (and is mostly compatible with OpenMapTiles).

Check out the [taxonomy chart](https://jawg.github.io/taxonomy/demo/?url=https://raw.githubusercontent.com/Qwant/qwant-basic-gl-style/master/style.json).

## Architecture

To mutualize some logic between Qwant Maps components, we use some config files to overwrite a few fields from the GL style template with [our map style builder](https://github.com/Qwant/map-style-builder).

Here are the main files of this repo and their usage:

* the `style.json` contains the GL style template. It looks like a regular GL style file but some fields will be overwritten at build time.
* the `tileschema_*.json` files contain the tile schema (in TileJSON format). Mostly useful for debugging purposes.
* the icons repository contains all the map icons. You can build a sprite and an icon-font from them with [our map style builder](https://github.com/Qwant/map-style-builder)
* the `i18n.yml` file contains the algorithm to create map in any language (well ... almost any). Most `name` fields specified in the `style.json` will be overwritten at build time with this config
* the `icons.yml` file contains the icons and text color to use for the points of interest. The `text-color` and the icons specified in the `style.json` will be overwritten at build time with this config

## Contribute

Thanks for considering to contribute ! :heart:

If you want to report bugs and make suggestions about Qwant Maps style, please use [Qwant Maps central repository](https://github.com/QwantResearch/QwantMaps) and provide screenshots.

If you know a bit about [Mapbox GL Style format](https://www.mapbox.com/mapbox-gl-js/style-spec) and want to send us some improvements, please read on ;)

You will need to install [Qwant Maps style builder](https://github.com/Qwant/map-style-builder).

#### Build

Build the style using our builder:

```
npm run build_all -- --style-dir=PATH/TO/YOUR/MAPSTYLE/FOLDER --conf=prod_conf.json --i18n=fr --webfont=true --icons=true
```

Then browse the `build` folder that has been created in your qwant-basic-gl-style folder: it contains
* the generated GL style. If you are not from Qwant, you may want to use the OpenMapTiles version to contribute: `style-omt.json`
* a few debug tools (check out the [map style builder](https://github.com/Qwant/map-style-builder) readme to learn more)

#### Test

[Our map style builder](https://github.com/Qwant/map-style-builder) can check a few things and give you hints about errors you may have in your style or config files.

`npm test -- PATH/TO/YOUR/MAPSTYLE/FOLDER`

## About icons to be used both in the map style and Qwant Maps UI

Icons present in the `icons` directory can be used both in the map and the webfont generated by [map style builder](https://github.com/Qwant/map-style-builder).
To add a new icon, a few constraints are to be followed:

1. Add two .svg files in `./icons`:
	* `{iconname}-11.svg` (with viewBox **23x23**, and a **10.5 px** border-radius)  
  Used to generate "sprite.png" (for default resolution)   
	Example: [`bar-11.svg`](./icons/bar-11.svg)
	* `{iconname}-15.svg` (with viewBox **27x27**, and a **12.5 px** border-radius)  
  Used to generate "sprite@2x.png" (for high DPI screens) and by map-style-builder to generate the webfont.
	Example: [`bar-15.svg`](./icons/bar-15.svg)

    > Note that the border-radius must be applied to a "rect" element, and that any "rect" element (including the icon border) will be ignored on webfont generation, to keep the main symbol "path" only. The (white) color of this path will also be removed in the webfont.

2. Update the rules in `icons.yml` mappings to associate the new icon with a POI class and/or subclass. (Mappings are defined with decreasing priority: the most specific rules should come first).
    > This second step is optional : if the new icon is not defined in the icons mappings, it will still be added to the webfont.

That's it!  
Update `qwant-basic-gl-style` dependency in [Erdapfel](https://github.com/QwantResearch/erdapfel) : the new icon will directly be used in the map style, and a CSS class `icon-{iconname}` will be defined.

PS: the new icons must follow the [Maki](https://labs.mapbox.com/maki-icons/guidelines/) guidelines, especially: no groups, flat icons, use fill instead of stroke, proper pixel alignment, 1px-wide strokes and all the units in "px".


## Our sprite icons

![sprite](https://qwant.github.io/qwant-basic-gl-style/sprite@2x.png)
