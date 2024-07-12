---
author: Hugo Authors
title: Hugo Usage
date: 2021-08-20
description: Hugo Usage
---
Using Notices functionality within this theme

The "Notices" shortcode enables you to call out pieces of information - sidebars, warnings, tips, etc.

To create a notice on a page, you can use the `notice` shortcode.  
You use the `notice` shortcode, with the first parameter being one of `note`, `info`, `tip`, and `warning`.  Then add a title for your 
note in quotes as the second parameter.  The inner body of the note can be whatever markdown you want to create.

The following shortcode syntax within a markdown doc:
```
{{%/* notice note "Note" */%}}
This is a standard "note" style.
{{%/* /notice */%}}
```
will render as:

{{% notice note "Note" %}}
This is a standard "note" style.
{{% /notice %}}

The other three variants follow.

{{% notice info "Info" %}}
Here is the "info" style.
{{% /notice %}}

{{% notice tip "Tip" %}}
Here is a "tip" variant of a notice.
{{% /notice %}}

{{% notice warning "Warning" %}}
Here is the "warning" flavor of a notice.
{{% /notice %}}


Also note that the content of a notice can contain anything you could put on a normal page - as shown below:

{{% notice tip "Complex Notices are Possible!" %}}
This is a notice that has a lot of various kinds of content in it.  

* Here is a bulleted list
* With more than one bullet 
    * And even more than one level

Code blocks are fine here, too....
```csharp
public void SayHello()
{
    Console.WriteLine("Hello, world!");
}
```
{{% /notice %}}


{{% notice tip "Productivity Booster!" %}}
If you're using VS Code for your editing, copy the `.vscode\clarity.code-snippets` file into a `.vscode` root folder on your repo.  This will enable you to type
`note` then `<tab>` then choose with up/down arrows which flavor notice you want, then `<tab>` again to provide a title, then `<tab>` to add your content!

![](../images/Note-Snippet.gif)

To use the snippet, you need to first **enable quickSuggestions for Markdown** (one time only):

1. Go to `Preferences->Settings` then search for `quickSuggestions`
1. Follow the link to Edit in settings.json
1. Toward the bottom of the file, paste in the following JSON:
```
"[markdown]":  {
    "editor.quickSuggestions": true
  }
```
4. Close and save the settings.
{{% /notice %}}

-----------------
Emoji can be enabled in a Hugo project in a number of ways. 
<!--more-->
The [`emojify`](https://gohugo.io/functions/emojify/) function can be called directly in templates or [Inline Shortcodes](https://gohugo.io/templates/shortcode-templates/#inline-shortcodes). 

To enable emoji globally, set `enableEmoji` to `true` in your site's [configuration](https://gohugo.io/getting-started/configuration/) and then you can type emoji shorthand codes directly in content files; e.g.

<p><span class="nowrap"><span class="emojify">🙈</span> <code>:see_no_evil:</code></span>  <span class="nowrap"><span class="emojify">🙉</span> <code>:hear_no_evil:</code></span>  <span class="nowrap"><span class="emojify">🙊</span> <code>:speak_no_evil:</code></span></p>
<br>

The [Emoji cheat sheet](http://www.emoji-cheat-sheet.com/) is a useful reference for emoji shorthand codes.

***

**N.B.** The above steps enable Unicode Standard emoji characters and sequences in Hugo, however the rendering of these glyphs depends on the browser and the platform. To style the emoji you can either use a third party emoji font or a font stack; e.g.

{{< highlight html >}}
.emoji {
  font-family: Apple Color Emoji, Segoe UI Emoji, NotoColorEmoji, Segoe UI Symbol, Android Emoji, EmojiSymbols;
}
{{< /highlight >}}

{{< css.inline >}}
<style>
.emojify {
	font-family: Apple Color Emoji, Segoe UI Emoji, NotoColorEmoji, Segoe UI Symbol, Android Emoji, EmojiSymbols;
	font-size: 2rem;
	vertical-align: middle;
}
@media screen and (max-width:650px) {
  .nowrap {
    display: block;
    margin: 25px 0;
  }
}
</style>
{{< /css.inline >}}

-----------------
Mathematical notation in a Hugo project can be enabled by using third party JavaScript libraries.
<!--more-->

In this example we will be using [KaTeX](https://katex.org/)

- Create a partial under `/layouts/partials/math.html`
- Within this partial reference the [Auto-render Extension](https://katex.org/docs/autorender.html) or host these scripts locally.
- Include the partial in your templates like so:  

```bash
{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}
```

- To enable KaTex globally set the parameter `math` to `true` in a project's configuration
- To enable KaTex on a per page basis include the parameter `math: true` in content files

**Note:** Use the online reference of [Supported TeX Functions](https://katex.org/docs/supported.html)

{{< math.inline >}}
{{ if or .Page.Params.math .Site.Params.math }}
<!-- KaTeX -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css" integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>
{{ end }}
{{</ math.inline >}}

### Examples

{{< math.inline >}}
<p>
Inline math: \(\varphi = \dfrac{1+\sqrt5}{2}= 1.6180339887…\)
</p>
{{</ math.inline >}}

Block math:
$$
 \varphi = 1+\frac{1} {1+\frac{1} {1+\frac{1} {1+\cdots} } } 
$$


------------------
This article offers a sample of basic Markdown syntax that can be used in Hugo content files, also it shows whether basic HTML elements are decorated with CSS in a Hugo theme.
<!--more-->

## Headings

The following HTML `<h1>`—`<h6>` elements represent six levels of section headings. `<h1>` is the highest section level while `<h6>` is the lowest.

# H1
## H2
### H3
#### H4
##### H5
###### H6

## Paragraph

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Images

### Local image, alt text as caption

The following image is located within the Hugo site. Because it has alt text but no title text, the caption is generated by the alt text.

![Jane Doe](../images/jane-doe.png)

### Remote image, specified caption

The following image is loaded from a remote URL. The alt text is the same (for screen readers and in cases when the image doesn't load) but because a separate title is provided, the title is used for the caption:

![Jane Doe](https://raw.githubusercontent.com/chipzoller/hugo-clarity/master/exampleSite/static/images/jane-doe.png "This is Jane Doe")

### Image with alt text and no caption

Alt text is always recommended for SEO, accessibility and in cases when images don't load. However, you don't necessarily always want an image to have a caption. In that case, use a title with one space:

![A building](../images/building.png " ")

## Blockquotes

The blockquote element represents content that is quoted from another source, optionally with a citation which must be within a `footer` or `cite` element, and optionally with in-line changes such as annotations and abbreviations.

#### Blockquote without attribution

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.

#### Blockquote with attribution

> Don't communicate by sharing memory, share memory by communicating.<br>
> — <cite>Rob Pike[^1]</cite>

[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports supports them out-of-the-box.

   Name | Age
--------|------
    Bob | 27
  Alice | 23

#### Inline Markdown within tables

| Italics   | Bold     | Code   |
| --------  | -------- | ------ |
| *italics* | **bold** | `code` |

## Code Blocks

#### Code block with backticks

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
<!-- this line is extraneous 2Error from server (Forbidden): deployments.apps is forbidden: User "chiptest" cannot create resource "deployments" in API group "apps" in the namespace "default" -->
</html>
```

#### Code block indented with four spaces

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

#### Code block with Hugo's internal highlight shortcode
{{< highlight html >}}
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## List Types

#### Ordered List

1. First item
2. Second item
3. Third item

#### Unordered List

* List item
* Another item
* And another item

#### Nested list

* Fruit
  * Apple
  * Orange
  * Banana
* Dairy
  * Milk
  * Cheese

## Other Elements — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.

----------------------
Lorem est tota propiore conpellat pectoribus de pectora summo. <!--more-->Redit teque digerit hominumque toris verebor lumina non cervice subde tollit usus habet Arctonque, furores quas nec ferunt. Quoque montibus nunc caluere tempus inhospita parcite confusaque translucet patri vestro qui optatis lumine cognoscere flos nubis! Fronde ipsamque patulos Dryopen deorum.

1. Exierant elisi ambit vivere dedere
2. Duce pollice
3. Eris modo
4. Spargitque ferrea quos palude

Rursus nulli murmur; hastile inridet ut ab gravi sententia! Nomine potitus silentia flumen, sustinet placuit petis in dilapsa erat sunt. Atria tractus malis.

1. Comas hunc haec pietate fetum procerum dixit
2. Post torum vates letum Tiresia
3. Flumen querellas
4. Arcanaque montibus omnes
5. Quidem et

# Vagus elidunt

<svg class="canon" xmlns="http://www.w3.org/2000/svg" overflow="visible" viewBox="0 0 496 373" height="373" width="496"><g fill="none"><path stroke="#000" stroke-width=".75" d="M.599 372.348L495.263 1.206M.312.633l494.95 370.853M.312 372.633L247.643.92M248.502.92l246.76 370.566M330.828 123.869V1.134M330.396 1.134L165.104 124.515"></path><path stroke="#ED1C24" stroke-width=".75" d="M275.73 41.616h166.224v249.05H275.73zM54.478 41.616h166.225v249.052H54.478z"></path><path stroke="#000" stroke-width=".75" d="M.479.375h495v372h-495zM247.979.875v372"></path><ellipse cx="498.729" cy="177.625" rx=".75" ry="1.25"></ellipse><ellipse cx="247.229" cy="377.375" rx=".75" ry="1.25"></ellipse></g></svg>

[The Van de Graaf Canon](https://en.wikipedia.org/wiki/Canons_of_page_construction#Van_de_Graaf_canon)

## Mane refeci capiebant unda mulcebat

Victa caducifer, malo vulnere contra dicere aurato, ludit regale, voca! Retorsit colit est profanae esse virescere furit nec; iaculi matertera et visa est, viribus. Divesque creatis, tecta novat collumque vulnus est, parvas. **Faces illo pepulere** tempus adest. Tendit flamma, ab opes virum sustinet, sidus sequendo urbis.

Iubar proles corpore raptos vero auctor imperium; sed et huic: manus caeli Lelegas tu lux. Verbis obstitit intus oblectamina fixis linguisque ausus sperare Echionides cornuaque tenent clausit possit. Omnia putatur. Praeteritae refert ausus; ferebant e primus lora nutat, vici quae mea ipse. Et iter nil spectatae vulnus haerentia iuste et exercebat, sui et.

Eurytus Hector, materna ipsumque ut Politen, nec, nate, ignari, vernum cohaesit sequitur. Vel **mitis temploque** vocatus, inque alis, *oculos nomen* non silvis corpore coniunx ne displicet illa. Crescunt non unus, vidit visa quantum inmiti flumina mortis facto sic: undique a alios vincula sunt iactata abdita! Suspenderat ego fuit tendit: luna, ante urbem Propoetides **parte**.

{{< css.inline >}}
<style>
.canon { background: white; width: 100%; height: auto; }
</style>
{{< /css.inline >}}

---------------------------

Hugo ships with several [Built-in Shortcodes](https://gohugo.io/content-management/shortcodes/#use-hugo-s-built-in-shortcodes) for rich content, along with a [Privacy Config](https://gohugo.io/about/hugo-and-gdpr/) and a set of Simple Shortcodes that enable static and no-JS versions of various social media embeds.
<!--more-->
---

## Instagram Simple Shortcode

<br>

---

## YouTube Privacy Enhanced Shortcode

{{< youtube izYiDDt6d8s >}}

<br>

---

## Twitter Simple Shortcode

{{< tweet user="SanDiegoZoo" id="1453110110599868418" >}}

See documentation https://gohugo.io/content-management/shortcodes/#tweet for more details

<br>

---

## Vimeo Simple Shortcode

{{< vimeo_simple 48912912 >}}

---

[Page bundles](https://gohugo.io/content-management/page-bundles/) are an optional way to [organize page resources](https://gohugo.io/content-management/page-resources/) within Hugo.

You can opt-in to using page bundles in Hugo Clarity with `usePageBundles` in your site configuration or in a page's front matter. [Read more about `usePageBundles`.](https://github.com/chipzoller/hugo-clarity#organizing-page-resources)

With page bundles, resources for a page or section, like images or attached files, live **in the same directory as the content itself** rather than in your `static` directory.

Hugo Clarity supports the use of [leaf bundles](https://gohugo.io/content-management/page-bundles/#leaf-bundles), which are any directories within the `content` directory that contain an `index.md` file. Hugo's documentation gives this example:

```text
content
├── about
│   ├── index.md
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── content2.md
│   │   ├── image1.jpg
│   │   ├── image2.png
│   │   └── index.md
│   └── my-other-post
│       └── index.md
│
└── another-section
    ├── ..
    └── not-a-leaf-bundle
        ├── ..
        └── another-leaf-bundle
            └── index.md
```

<blockquote>
In the above example `content` directory, there are four leaf
bundles:

**about**: This leaf bundle is at the root level (directly under
    `content` directory) and has only the `index.md`.

**my-post**: This leaf bundle has the `index.md`, two other content
    Markdown files and two image files. **image1** is a page resource of `my-post`
    and only available in `my-post/index.md` resources. **image2** is a page resource of `my-post`
    and only available in `my-post/index.md` resources.

**my-other-post**: This leaf bundle has only the `index.md`.

**another-leaf-bundle**: This leaf bundle is nested under couple of
    directories. This bundle also has only the `index.md`.

_The hierarchy depth at which a leaf bundle is created does not matter,
as long as it is not inside another **leaf** bundle._
</blockquote>

### Advantages to using page bundles

The image below is part of the bundle of this page, and is located at `content/post/bundle/building.png`. Because it's within this page's bundle, the markup for the image only has to specify the image's filename, `building.png`.

![A building](building.png)

If you ever change the name of the directory in which this Markdown file and the image reside, the reference to the image would not need to be updated.

In addition to more cleanly organizing your content and related assets, when using page bundles, **Hugo Clarity will automatically generate markup for modern image formats**, which are smaller in file size.

For instance, when you reference an image like `building.png`, Hugo Clarity will check to see if the same image (based on filename) exists in [WebP](https://en.wikipedia.org/wiki/WebP), [AVIF](https://en.wikipedia.org/wiki/AVIF) or [JXL](https://en.wikipedia.org/wiki/JPEG_XL) formats. If you inspect the image above, you'll see a `<source>` element for `building.webp`, because that file is also present. Hugo Clarity will only include the markup if these images exist.

Browsers that [support these formats and the `<picture>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture#the_type_attribute) will load them, while browsers that do not will fall-back to the default image. [Read more about this process.](https://github.com/chipzoller/hugo-clarity#support-for-modern-image-formats)

Finally, note that page assets can be further managed and refined [within the page's front matter](https://gohugo.io/content-management/page-resources/#page-resources-metadata) if you wish, and are not limited to images alone.

### Disadvantages to using page bundles

Page resources in a bundle are only available to the page with which they are bundled &#8212; that means you can't include an image with one page and then reference it from another.

Images that are being used in multiple places are more appropriate for your [Hugo `assets` directory](https://gohugo.io/hugo-pipes/introduction/). Unlike files in the Hugo `static` directory, files in the `assets` directory can be run through Hugo Pipes, which [includes image processing](https://gohugo.io/content-management/image-processing/).
