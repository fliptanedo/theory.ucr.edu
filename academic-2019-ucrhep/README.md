# UCR hep-ph group website Readme

## Credit

This is a modified version of George Cushen's *Academic* theme for Hugo. Flip Tanedo modified this theme starting in 2017 for the University of California, Riverside Particle Theory group website. See the default *Academic* kickstart readme below.

## Directory Structure
```
www_2019
+--academic-2019-flipucr
+-- academic-2019-ucrhep
    └-- README.md (this file)
    └-- [other files]
+-- academic-flip
+-- academic-kickstart
README.txt
```


## Edits from the default themes

These are notes on how the Academic kickstart

* Copied from old build:
  - `/static/img/` directory containing group photos, etc.
  - `/static/css/` directory containing `UCRhep.css`
  - `/data/fonts/flip.toml` with font metadata (set `font = "flip"` in `config.toml`):
  - `/layouts/` directory with custom formatting and shortcodes ... but these needed to be updated

  ```toml
  # Font style metadata
  name = "flip"

  # Optional Google font URL
  google_fonts = "Montserrat:400,700|Open+Sans|Roboto+Mono|Raleway"

  # Font families
  heading_font = "Montserrat"
  body_font = "Raleway"
  nav_font = "Roboto"
  mono_font = "Roboto Mono"

  # Font size
  font_size = "20"
  font_size_small = "18"
```

* Many other copied options in `config.toml` (note there have been some updates to the Academic theme, notably in the icon packs)

* Favicon: observe that the `/themes/layouts/partials/header.html` template has the following:
```html
  <link rel="icon" type="image/png" href="{{ "/img/icon.png" | relURL }}">
  <link rel="apple-touch-icon" type="image/png" href="{{ "/img/icon-192.png" | relURL }}">
```
This means that need to place an `icon.png` and an `icon-192.png` file in the `/img/` directory for the favicon. (Do *not* modify the template: there's no need, and you should never directly edit the source templates anyway.)

* Note: the latest version of Hugo uses Bootstrap 4. The previous version of the group website used Bootstrap 3. This means that all of my partials have incorrect grids. This can be tricky to sort out and has forced me to be careful with indentation. Use `\themes\...\custom.html` as a template.




# Academic Kickstart (Default Readme)

**Academic** is a framework to help you create a beautiful website quickly. Perfect for personal, student, or academic websites. [Check out the latest demo](https://themes.gohugo.io/theme/academic/) of what you'll get in less than 10 minutes or [view the documentation](https://sourcethemes.com/academic/docs/).

**Academic Kickstart** provides a minimal template to kickstart your new website by following the simple steps below.

[![Screenshot](https://raw.githubusercontent.com/gcushen/hugo-academic/master/academic.png)](https://github.com/gcushen/hugo-academic/)

## Getting Started

The following two methods describe how to install in the cloud using your web browser and how to install on your PC using the Command Prompt/Terminal app.

### Quick install using your web browser

1. [Install Academic with Netlify](https://app.netlify.com/start/deploy?repository=https://github.com/sourcethemes/academic-kickstart)
    * Netlify will provide you with a customizable URL to access your new site
2. On GitHub, go to your newly created `academic-kickstart` repository and edit `config.toml` to personalize your site. Shortly after saving the file, your site will automatically update
3. Read the [Quick Start Guide](https://sourcethemes.com/academic/docs/) to learn how to add Markdown content. For inspiration, refer to the [Markdown content](https://github.com/gcushen/hugo-academic/tree/master/exampleSite) which powers the [Demo](https://themes.gohugo.io/theme/academic/)

### Install on your PC

Prerequisites:

* [Download and install Git](https://git-scm.com/downloads)
* [Download and install Hugo](https://gohugo.io/getting-started/installing/#quick-install)

1. Clone (or [Fork](https://github.com/sourcethemes/academic-kickstart#fork-destination-box) or [download](https://github.com/sourcethemes/academic-kickstart/archive/master.zip)) the *Academic Kickstart* repository with Git:

       git clone https://github.com/sourcethemes/academic-kickstart.git My_Website

    *Note that if you forked Academic Kickstart, the above command should be edited to clone your fork.*

2. Initialize the theme:

       cd My_Website
       git submodule update --init --recursive

3. View your new website:

       hugo server

    Now you can go to [localhost:1313](http://localhost:1313) and your new Academic powered website should appear.

4. Read the [Quick Start Guide](https://sourcethemes.com/academic/docs/) to learn how to add Markdown content, customize your site, and deploy it.

## License

Copyright 2017 [George Cushen](https://georgecushen.com).

Released under the [MIT](https://github.com/sourcethemes/academic-kickstart/blob/master/LICENSE.md) license.

[![Analytics](https://ga-beacon.appspot.com/UA-78646709-2/academic-kickstart/readme?pixel)](https://github.com/igrigorik/ga-beacon)
