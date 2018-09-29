# Flip's website
by Flip Tanedo (flip.tanedo@ucr.edu)

This is the late 2018 version of my website.

These are notes on how I modified the May 2018 version of `Hugo Theme Academic` (George Cushen) to create my personal website. See the file `AcademicREADME.md` for information about Hugo.

This version updates `academic-flip/` from early 2018.

# Description of Modifications

Stylistically, the main update from the `Academic` theme is to include my Feynman diagram footer, which requires some hacks to the style sheets.

Modifying the footer ends up being a little subtle because of the way the footer margins and padding are tied to the font size. The font size changes (responsive design), which means the footer parameters change with screen size. In turn, this makes it difficult to place graphical footer elements.

The biggest change for this iteration is that I am now hosting under the `theory.ucr.edu` subdomain rather than separately on `physics.ucr.edu`. The theory domain is a separate Hugo website. The unified project runs two separate Hugo engines in the following directory structure:

```
www_2019
+-- academic-2019-flip
+-- academic-2019-ucrhep
    └-- README.md (this file)
    └-- [other files]
+-- academic-flip
+-- academic-kickstart
+-- www2019
    └-- flip
README.txt
```

The only tricky thing was to put in the hugo build command. The GitHub repository has this hugo project as a subdirectory (see directory structure above) and outputs to a deploy directory that is on the same level (not a subdirectory) of the hugo project. You thus have to tell Netlify to run hugo in a subdirectory.
```
Build command: cd academic-2019-flip ; hugo ; cd ..
Publish directory: www2019
```

# My work environment

I used **netlify** to kickstart my fork of Hugo Theme Academic. This should make it easier to have my university url point to the netlify repository.

I am using the **GitHub** OS X interface to take care of synchronizing with GitHub. I use `Atom` to edit files.

I have a separate folder `\Graphic Sources` that includes source files for Affinity Designer graphics.

# From Academic Kickstart to Flip's page

Here are my instructions (written primarily for myself) for going from the Hugo Academic Kickstart to the current iteration of the site. I start with the Academic Kickstart and then ported over elements of the previous version (updating as necessary).

Download the kickstart: https://github.com/sourcethemes/academic-kickstart/archive/master.zip

Download the theme (place this in `/themes/academic/`): https://github.com/gcushen/hugo-academic/archive/master.zip

## 0. Transfer Assets

The first part is to transfer the `static` and `data` folders. Ideally you could also transfer `layouts`, but this is where we have the most edits and where we're most sensitive to theme updates. In section 2 we go through all of the edits.

Transfer `layouts\shortcodes`, which contains `flipemail.html`, `googlecalendar.html`, `twitter.html`.

## 1. Initial Set Up

Here are the steps that I'm taking to go from the clean example site to one that is ready for hacking.

Delete:

* `\content\home\*` *except* `about.md`, `contact.md`, `hero_carousel.md` and `teaching.md` (template).

* `\content\post` , `\content\publication`, `\content\talk`

* Remove the fluff in `config.toml` ... there's a lot fo commentary. They're good to have handy, but it's distracting when the `config` file gets longer. I'll make a backup and then streamline. It's worth doing this carefully in case `config.toml` has been updated in Hugo Academic.


## 2. Footer and Main Page Design

The biggest change is the footer. You can just copy the `layouts` folder, but let's document this carefully. Strategy: create a `<div id="THECONTENT">` to give a white canvas. The `<body>` tag behind it will be dark gray to match the header and footer so that "overscrolling" will not look strange.

1. (A) Make folder `/layouts/partials/` to include template overwrites. (B) Copy the following files from the corresponding `template` folder: `footer_container.html`, `footer_section.html` and `navbar.html`.

2. Add the following line to the bottom of `navbar.html`:
```html
<!-- FLIP 2019 -->
<div id="THECONTENT"> <!-- closed in footer_container.html; see flip2019.css -->
  <!-- /FLIP 2019 -->
```
This *opens* a `div` at the start of each page. We should close it.

3. Add the following line to the bottom of `footer_container.html`:
```html
<!-- FLIP 2019 -->
</div> <!--  ends id=#THECONTENT from navbar.html; see flip2019.css -->
<!-- /FLIP 2019 -->
```
This *closes* the `div` that we started in step 2.

4. [this step should be taken care of when you copied over the `static` folder.] Add folder: `/static/css/` Insert file `flip2018.css` This introduces a bunch of kludges. For now, the main point of this file is to contain:

```css
footer {
  /* font-family: 'Raleway', sans-serif; */
  background-color:#333333;
}

#botbar1{
	background-color:#C1BBAB;
	margin: 0;
	padding: 0;
  position:relative;
  top: 85px;
	left: 0px;
	z-index: 9;
	height: 7px;
	width: 100%;
}

body{
  background-color:#333333;
}
```

5. Load `flip2019layout.css` by updating the `custom_css` line in `config.toml`:

```
custom_css = ['flip2019layout.css']
```

At this point, you should have a dark gray footer.


Now let's get to the gray line separating the main content and the footer. This is what we call `#botbar1`.

6. In `\layouts\partials\footer_container.html`, insert a `#botbar` div at the top of the file:

```html
<!-- FLIP 2019 -->
  <div id="botbar1"></div>
  <div style="position: relative; width: 0; height: 0; z-index: 200;">
    <div id="feynmanfoot" style="background-image:url('{{ .Site.BaseURL }}/img/{{ .Site.Params.footmark }}');"></div>
  </div>
<!-- /FLIP 2019 -->
  <div class="container">
```

The bottom line should show up at the bottom of the page. You should play with the `#botbar1` top margin in `flip2019layout.css` to see where this line ends up. There's something pressing and subtle to sort out first, though:

7. Fix the `footer` style so that it's written in `px` rather than `rem` or `em`. The problem with `rem` (relative to root) or `em` is that when the font size changes from responsive design, the padding will shift which will force you to re-calculate the top shift of `#botbar`. [You shouldn't have to do this if you just copied over the `static` folder.]


```css
footer {
  /* font-family: 'Raleway', sans-serif; */
  background-color:#333333;
  /* Fixing academic.css line 979  */
  margin: 64px 0 0;
  padding: 32px 0;
}

#botbar1{
	background-color:#C1BBAB;
	margin: 0;
	padding: 0;
  position:relative;
  top: -35px;
	left: 0px;
	z-index: 9;
	height: 7px;
	width: 100%;
}
```

8. Now insert Feynman footer. Insert iamge into `/static/img/layout/feymmanfooter.png`. Here's what seems to work for `flip2019layout.css`:

```css
#feynmanfoot{
	background-size: 250px 200px;
	background-repeat:no-repeat;
	background-position:left top;
  pointer-events: none;
	margin: 0;
	padding: 0;
  position: relative;
  /* bottom: -100px;  */ /* if you place div above footer */
  bottom: 205px;
  left: 40vw;
	z-index: 200;
	height: 200px;
	width: 250px;
	text-align: right;
}
```

But now we have a problem: even though the decorative lines span the entire width of the browser, but the footer (contained in `<div class="container">`) has a margin. This is annoying. It comes from an update of the Academic style that swapped the nesting of `<div class="container">` and `<footer>`. We have to hack `footer_container.html` and `footer_section.html` in order to go back to the original order.

And here's the top of `footer_container.html`:

```html
<!-- FLIP 2019 -->
<footer class="site-footer">
  <div id="botbar1"></div>
  <div style="position: relative; width: 0; height: 0; z-index: 200;">
    <div id="feynmanfoot" style="background-image:url('{{ .Site.BaseURL }}/img/{{ .Site.Params.footmark }}');"></div>
  </div>
<!-- /FLIP 2019 -->
  <div class="container">
    <!-- Contains footer information -->
    {{ partial "footer_section.html" . }}
  </div>
<!-- FLIP 2019 -->
</footer>
<!-- /FLIP 2019 -->
```

Be sure to comment out the `<footer>` tags in `footer_section.html`:
```HTML
<!-- FLIP 2019 -->
<!-- Comment out the <footer> tags -->
<!-- <footer class="site-footer"> -->
<!-- /FLIP 2019 -->
```
And similarly for the closing tag.


9. Here's a minor follow up: let's make the footer section have three columns.

Footer triple column, inspired by [hugo-initio](https://themes.gohugo.io/theme/hugo-initio/) theme by Miguel Simoni. This is a fairly simple one, use a *bootstrap* grid. We have to modify `\layouts\partials\footer_section.html`:

The `div` layout is as follows:  
`footer site-footer` > `div botbar1` > `div feynmanfoot` > `div` (spacer) > `div container`  

In the default academic theme, the next layer is a `p powered-by`. What we're going to do is replace that with a `div row` > `div col-sm-4`.

Now we should define some logos to put into the columns. I'm adding the information under the `#FOOTER PARAMETERS` part of `config.toml`.

Here's what it looks like:

`footer_section.html`:

```html
<!-- FLIP 2019 -->
<div class="row" id="footer-columns">
  <div class="col-md-4" id="footer-col-1">
    <img src="{{ $.Site.BaseURL }}img/{{ $.Site.Params.stacklogo }}">
  </div>
  <div class="col-md-4" id="footer-col-1">
    {{ $.Site.Params.legalblurb }}
  </div>
  <div class="col-md-4" id="footer-col-1">
    Powered by the
    <a href="https://gohugo.io" target="_blank" rel="noopener">Hugo</a> engine
    <br/ >
    Adapted from <a href="https://sourcethemes.com/academic/" target="_blank" rel="noopener">Academic</a>.
    <br/ >
    {{ with .Site.Copyright }}{{ . | markdownify}} {{ end }}
    </div>
</div>
<!-- /FLIP 2019 -->
```

config.toml:

```
  # FOOTER PARAMETERS
  footmark = "layout/feynmanfooter.png"
  stacklogo = "logo/UCR_PhysicsAndAstro_Logo_fordarkBG3.png"
  legalblurb = "All opinions expressed here are my own and do not necessarily represent those of any other agencies or groups."
```

flip2018layout.css:  

```
/*  FOOTER 3 Column  */

#footer-columns{
  padding-top: 35px;
  margin-right: 20px;
  margin-left: 20px;
  font-size: .75rem;
  font-family:Lato;
}

#footer-col-1{
  padding-bottom: 25px;
}

#footer-columns img{
  max-width: 200px;
  opacity: .5;
}
```


## 3. Personalization

1. `config.toml`: `avatar = "profile/Flip600_sq.jpg"` (and upload the respective file)

2. In `header.html` (theme default) we have the following: So insert `icon.png` and `icon-192.png` as appropriate.
<!--add `\static\favicon.ico` In -->

```html
  <link rel="icon" type="image/png" href="{{ "/img/icon.png" | relURL }}">
  <link rel="apple-touch-icon" type="image/png" href="{{ "/img/icon-192.png" | relURL }}">
```

(You don't need to make edits to `header.html` for this.)

3. `cryptedemail.css` is a hack for displaying e-mail addresses in a way that isn't too easy for bots to scrape. Include it in the `config.toml` list of css files. It goes with `\layouts\shortcodes\flipemail.html` which provides a simple way to insert e-mail addresses. You'll also want to break up the e-mail address in `config.toml`:

```
  # email = "test@example.org"  # using cryptedmail
  email1 = "flip.tanedo" # using cryptedmail
  email2 = "ucr" # using cryptedmail
  email3 = "edu" # using cryptedmail
```

Commenting out the old e-mail removes that line in the `contact` widget. We'll use the [cryptedmail trick](https://stackoverflow.com/a/41566570). Create a `contact.html` widget in `\layouts\partials\widgets\` to

```html
<!-- FLIP UPDATE -->
<!-- from: https://stackoverflow.com/a/41566570 -->

      {{ with $.Site.Params.email1 }}
      <li>
        <i class="fa-li fa fa-envelope fa-2x" aria-hidden="true"></i>
        <span id="person-email" itemprop="email">
        <!-- {{- if $autolink }}<a href="mailto:{{ . }}">{{ $.Site.Params.email2 }}</a>{{ else }}{{ . }}{{ end -}} -->
          <a data-name="{{ $.Site.Params.email1 }}" data-domain="{{ $.Site.Params.email2 }}" data-tld="{{ $.Site.Params.email3 }}" href="#" class="cryptedmail" onclick="window.location.href = 'mailto:' + this.dataset.name + '@' + this.dataset.domain + '.' + this.dataset.tld"></a>
        </span>
      </li>
      {{ end }}

<!-- /FLIP UPDATE -->
```

4. Added `flip2018.css` (introduces `comment` class, also Google calendar settings), and `flipaboutme.css` which modifies the `about` widget.

5. New `about.html` in `\layouts\partials\widgets`. I moved around a bunch of stuff.


## 4. Fonts 'n stuff

These should already be taken care of when we copied over the `static` and `data` folders.

1. Insert `\data\fonts\flipfont.toml` and update the `config.toml` file to include

```toml
font = "flipfont"
```

To fix the e-mail font, put this in `flip2018.css`:

```css
#person-email{
  font-family: 'Titillium Web', sans-serif;
}

.style-email{
  font-family: 'Titillium Web', sans-serif;
}
```

Use `style-email` in the `flipemail.html` shortcode:

```html
<!-- in conjunction with cryptedemail.css -->
<a
  data-name="{{ .Get "first" }}"
  data-domain="{{ .Get "domain" }}"
  data-tld="{{ .Get "last" }}"
  href="#"
  class="cryptedmail"
  onclick="window.location.href = 'mailto:' + this.dataset.name + '@' + this.dataset.domain + '.' + this.dataset.tld"
  class="style-email">
</a>
```

## 5. Colors

These should already be taken care of when we copied over the `static` and `data` folders.


1. Paste in `data\themes\fliptheme.toml` (this is a color theme)

2. Include it in `config.toml`

```toml
color_theme = "fliptheme"
```


## 6. Tweaking the nav bar

In an earlier version (early 2018) I was frustrated that the navbar  defaults to being "tap to menu" for sizes below 1200px.  This is called the **breakpoint** of the navigation bar. 1200px exceedingly generous if you only have a few sections on your main page or if you want to use the navbar to navigate to other pages. The `README.md` file for that version has a long discussion about this.

The new version of Academic supports Bootstrap 4, which should make this easier.

* [bootstrapius](https://bootstrapious.com/p/bootstrap-navbar#navbar-collapse)
* [love2dev](https://love2dev.com/blog/bootstrap-navbar/)

From bootstrapius:
> The navbar collapse is the component which wraps the remain components of the navbar, such as navbar menu, navbar buttons, forms... etc. In other words, it wraps all navbar components except `.navbar-header`.

> It's defined by the two classes `.collapse navbar-collapse`. This box should have an `id`, this id must be identical to the toggle button `data-target=""` value.

> Contrary to the navbar toggle button, *navbar collapse is visible by default on larger screens and hidden on smaller ones*. On smaller screens, it is switched from hidden to visible and vice versa by clicking navbar toggle button.

The thing I'm looking for is called the **breakpoint**. Use that in my search queries. Maybe [this](https://stackoverflow.com/questions/19827605/change-bootstrap-navbar-collapse-breakpoint-without-using-less). The "[2018 update](https://stackoverflow.com/a/36289507/4812646)" answer is the one that I want:

> Changing the navbar breakpoint is easier in Bootstrap 4 using the navbar-expand-* classes:

```html
<nav class="navbar fixed-top navbar-expand-sm">..</nav>
```

The options are

* `navbar-expand-sm`: mobile menu on xs screens <576px
* `navbar-expand-md`: mobile menu on sm screens <768px
* `navbar-expand-lg`: mobile menu on md screens <992px
* `navbar-expand-xl`: mobile menu on lg screens <1200px
* `navbar-expand`: never use mobile menu
* *(no expand class)* = always use mobile menu

See [this example](https://www.codeply.com/go/n4Pn4aB695). So what we really need is to hack is `\layouts\partials\navbar.html`, which we have already modified when we added the `<div id="THECONTENT">` line at the end of the file.

On the top line, modify to:
```html
<!-- FLIP 2019 -->
<!-- hacking breakpoint for navbar -->
<nav class="navbar navbar-light fixed-top navbar-expand-md py-0" id="navbar-main">
<!-- <nav class="navbar navbar-light fixed-top navbar-expand-lg py-0" id="navbar-main"> -->
<!-- /FLIP 2019 -->
```

That was *way* easier than the hack we had to use under Bootstrap 3. Thanks, George Cushen, for updating.

## 7. More formatting tweaks

### Tweaking job title font

In `/layouts/partials/widgets/about.html` around line 19 (`<div class="portrait-title">`), did a minor font change for the job title and organization:

```html
      <div class="portrait-title">
        <h2 itemprop="name">{{ $.Site.Params.name }}</h2>
        {{ with $.Site.Params.role }}
          <!-- <h3 itemprop="jobTitle">{{ . }}</h3> -->
          <h3 itemprop="jobTitle" style="font-family:Lato;">{{ . }}</h3>
        {{ end }}
```

* Make the navbar opaque. Opacity is in the `rgba` css designator. This seems to have changed since the early 2018 version, perhaps because of bootstrap.

For this we need `flipnav.css` to define a new class:
```css
.navbar-flip {
  background-color:rgba(0, 0, 0, 0.75) !important;
}
```
Then go back to `navbar.html` and make sure that this class is included in the navbar:
```HTML
<nav class="navbar navbar-flip navbar-light fixed-top navbar-expand-md py-0" id="navbar-main">
```

### Footer: alternating color issue (work in progress)

Widget pages (like the home page) use the `nth-of-type` CSS structure to create alternating styles. The final widget may end up with a gray background.

Here's how `academic.css` does it:

```css
.home-section {
  background-color: {{ .Get "home_section_odd" }};
  padding: 110px 0 110px 0;
  animation: intro 0.3s both;
  animation-delay: 0.15s;
}

.home-section:first-of-type {
  padding-top: 50px;
}

.home-section:nth-of-type(even) {
  background-color: {{ .Get "home_section_even" }};
}
```

We have to adapt the footer background in case an "even" numbered widget is last and thus has a gray background. This is something we introduced in the `#THECONTENT` divider and is controlled in flip2019layout.css. This also controls all the even numbered guys.

I can fix this in flip2018.css by removing the transparency:

```css
/* to make "white bg" transparent */
.home-section {
  /* background-color: transparent; */
  /* background-color: {{ .Get "home_section_odd" }}; */
  z-index: 1;
}

```
... but this gits rid of the transparent background and doesn't really solve the problem. It just means that you can hard code the background color by setting what `#THECONTENT` color should be.

I think the way to fix this is to apply some Hugo magic to `navbar.html` and have the `<div id="THECONTENT">` line (the last one) set a background color that depends on the number of widgets. Here's a pretty good attempt:

```html
<!-- BASED ON widget_page.html -->
  {{ if .IsHome }}
    {{ .Scratch.Set "section" "home" }}
  {{ else }}
    {{ .Scratch.Set "section" .Section }}
  {{ end }}
  {{ $section := .Scratch.Get "section" }}

  {{ $page := where (where .Data.Pages "Section" $section) ".Params.active" "!=" false }}
  {{ $pageCount := len $page }}
  {{ if modBool $pageCount 2}}
    <div id="THECONTENT" style="background-color: rgb(247, 247, 247);">
  {{ else }}
    <div id="THECONTENT" style="background-color: rgb(255, 255, 255);">
  {{ end }}
```

The problem with this one is that (1) I still need fix the widget background colors (they're incompatible with transparency) and (2) I get a weird extra space at the top. Hugo seems to be inserting an implicit `<br>` or something.

**For now** I think the best answer is to just have odd numbers of widgets. Bit of a shame, but this is something that I can "deep dive" into when there's time.

## 8. Content

### About Widget

We'd like to put a watermark.

In flip2018.css:  

```css
/* to make "white bg" transparent */
.home-section {
  background-color: transparent;
  z-index: 1;
}

#watermark{
	/*background-image:url('http://physics.ucr.edu/~flip/images/BG_Bundle2.jpg');*/
	background-size: 700px 350px;
	background-repeat:no-repeat;
	background-position:left top;
	margin: 0;
	padding: 0;
	position: absolute;
	top: 30px;
	left: 0px;
  pointer-events: none;
	/*z-index: 0;*/
	height: 100%;
	width: 100%;
	text-align: right;
	opacity: 0.1;
}
```

Now hack the top of `layouts\partials\navbar.html` (which we already modified for the navbr):  

```html
<!-- FOR WATERMARK -->
<!-- Put it here for convenience -->
<div id="watermark" style="background-image:url('{{ $.Site.BaseURL }}/img/{{ .Site.Params.watermark }}');"></div>
<!--  -->
<nav class="navbar navbar-default navbar-fixed-top" id="navbar-main">
```

Note: in past iterations I hacked `header.html`, but this is annoying since it's another template default file that we're over-riding for a small reason. Instead, it's easy to see that `header.html` is always immediately followed by `navbar.html` and, further, that the place to insert the watermark is just after the `<body>` opening, which is the last line of `header.html`. Thus placing the watermark `<div>` as the first line of `navbar.html` gives the identical effect.


### CV Widget

Create a `/layouts/partials/widgets/CV.html` file based on the `academic/layouts/partials/widgets/custom.html` template. Create a `/content/home/CV.md` file based on the `/content/home/teaching.md` tempalte.

In `CV.md`, first point to the `CV.html` widget template: `widget = "CV"`.

Note: I'm only editing the part of the widget that assumes that there is a title. (If `CV.md` does not have a title, then the page will not render!)

#### Add "download pdf" option:

In `CV.html` under `<div class="col-xs-12 col-md-4 section-heading">`:

```html
    <p>
    <a class="btn btn-primary btn-outline btn-xs" href="{{ $page.Params.cv_pdf }}">
      Download Complete CV
    </a>
    </p>
```

In the header of `CV.md`:

```md
# CV location
cv_pdf = "./files/Tanedo.pdf"
```

In the file structure: `/static/files/Tanedo.pdf`.


#### The group logo

In `flip2018.css`:  

```css
/*  for CV WIDGET  */

.sidebarpic{
  /* sets max size  */
  width: 250px;
  height: 250px;
  text-align: center;
  margin: 0 auto;
}
```

In `CV.md`:  

```md
# Group Logo
group_logo = "./img/logo/UCRHEP_03.png"
```

In `CV.html`:  

```html
    {{ if $page.Params.group_logo }}
    <p>
      <img class="sidebarpic" src="{{ $page.Params.group_logo }}">
      <meta itemprop="image" content="{{ $page.Params.group_logo }}">
    </p>
    {{ end }}
```

Note that I was a bit more clever with the go `if` statement here. That's probably good practice.


#### Filling it in

`CV.html`:  

```html
  <div class="col-12 col-lg-8">
    {{ $page.Content }}

    <!-- FLIP 2019  -->
    <div class="row">

      {{ with $page.Params.interests }}
      <div class="col-md-5">
        <h3>{{ i18n "interests" | markdownify }}</h3>
        <ul class="ul-interests">
          {{ range .interests }}
          <li>{{ . | markdownify }}</li>
          {{ end }}
        </ul>
      </div>
      {{ end }}

      {{ with $page.Params.education }}
      <div class="col-md-7">
        <h3>{{ i18n "education" | markdownify }}</h3>
        <ul class="ul-edu fa-ul">
          {{ range .courses }}
          <!-- <li> -->
            {{ if .logo }}
              <img src="{{ $.Site.BaseURL }}img/{{ .logo }}" style="height:1rem; float: left; padding-right: 10px;">
            {{ else }}
              <i class="fa-li fas fa-graduation-cap"></i>
            {{ end }}
            <div class="description">
              <!-- <p class="course">{{ .course }}{{ with .year }}, {{ . }}{{ end }}</p>
              <p class="institution">{{ .institution }}</p> -->
              {{ .course_short }}
              {{ with .institution_short }}, {{ . }}{{ end }}
              {{ with .year }}({{ . }}){{ end }}
              <br />
            </div>
          <!-- </li> -->
          {{ end }}
        </ul>
      </div>
      {{ end }}

    </div>
    <!-- /FLIP 2019 -->
  </div>
```

### Other widgets
I think the `research`, `contact`, `students`, and `teaching` widgets are straightforward.

### Research Widget

This would be great for a carousel. Cushen [recommends](https://github.com/gcushen/hugo-academic/issues/399): [Slick](http://kenwheeler.github.io/slick/).

**Setting Up Slick**:  
* Note: header.html has the line that ranges over `custom_css`, whereas footer.html ranges over `custom_js`. This is relevant for slick.

* `slick.css` needs to be in the `<head>`, which works if you put it in the `custom_css` list in `config.toml`.

* `slick.js` needs to be before the closing `</body>` tag, which means that having it in the footer is fine, *except*...

* ... that it has to be called *after* **jQuery** (version > 1.7). `academic` already calls jQuery in footer.html, and further that the `custom_js` scripts are loaded *after* jQuery is called. We should be safe putting `slick.js` in the `custom_js` list of `config.toml`. (You'll need to make a `\static\js\` folder if you haven't already.)  **Update**: the current version of **jQuery** being called in `academic` is an old one. We'll have to call a newer one. ((Hopefully all this stuff gets cleaned up with the next version of `academic`.))

* A few others: in the `css` directory, include `ajax-loader.gif` and the slick `fonts` directory. This clutters things up a little, but we can clean it up later. (I'm not even sure how necessary this is, but it was throwing up errors in my browser.)

* I made a `flipslick.css` and a `flipslick.js` are based on pieces of code in the slick examples.

`flipslick.css` is: (use `.slider`'s `margin` to change the spacing above and below the slider.)

```css
.slider {
        width: 90%;
        margin: 10px auto;
    }

    .slick-slide {
      margin: 0px 20px;
    }

    .slick-slide img {
      width: 100%;
    }

    .slick-prev:before,
    .slick-next:before {
      color: black;
    }

    .slick-slide {
      transition: all ease-in-out .3s;
      opacity: .2;
    }

    .slick-active {
      opacity: .5;
    }

    .slick-current {
      opacity: 1;
```

* First, make a local copy of `https://code.jquery.com/jquery-2.2.0.min.js`, put it in the `\js\` folder and make sure `config.toml` calls it *before* `slick.js`.

* Then make a `flipslick.js` in the same folder and make sure `config.toml` calls it *after* `slick.js`. The content of `flipslick.js` are the details of the slider. We'll use


```js
$(document).on('ready', function() {
  $('.single-item').slick({
    dots: true,
    infinite: true,
    centerMode: true}
  );
});
```

* So far so good. You can now test this by inserting the following into the `research.html` template:

```html
<!--  -->

    <!-- <div class="center slider"> -->
    <div class="single-item slider">
    <div>
      <img src="http://placehold.it/350x100?text=1">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=2">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=3">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=4">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=5">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=6">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=7">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=8">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=9">
    </div>
    <div>
      <img src="http://placehold.it/350x100?text=10">
    </div>
  </div>



<!--  -->
```

* If that worked, we'll now have to replace that test code with **hugo** commands that scan over `research.md`.

Here's how I did it:

```html
<!--  -->

    <div class="single-item slider">
    {{ with $page.Params.research }}
      {{ range .projects }}
      <div>

        <img src="{{ $.Site.BaseURL }}img/{{ .image }}">
        <!-- <img src="{{ .image }}"> -->

        <div style = "font-family:Lato; font-size: .7rem;">
        {{ if .caption }}
          {{ .caption | markdownify }}
        {{ end }}
        </div>
      </div>
      {{ end }}
    {{ end }}
  </div>


<!--  -->
```

* Here's the bonus: make it a shortcode
