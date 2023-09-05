HTML is the raw data that a webpage is built out of.
All the text, links, cards, lists, and buttons are created in HTML

CSS is what adds _style_ to those plain elements. HTML puts information on a webpage, and CSS positions that information, gives it color, changes the font, and makes it look great!

# Elements and Tags
### [Introduction](https://www.theodinproject.com/lessons/foundations-elements-and-tags#introduction)

HTML (HyperText Markup Language) defines the structure and content of webpages. We use HTML elements to create all of the paragraphs, headings, lists, images, and links that make up a typical webpage.

### [Lesson overview](https://www.theodinproject.com/lessons/foundations-elements-and-tags#lesson-overview)

This section contains a general overview of topics that you will learn in this lesson.

- Explain what HTML Tags are
- Explain what HTML elements are

### [Elements and tags](https://www.theodinproject.com/lessons/foundations-elements-and-tags#elements-and-tags)

Almost all elements on an HTML page are just pieces of content wrapped in opening and closing HTML tags.

Opening tags tell the browser this is the start of an HTML element. They are comprised of a keyword enclosed in angle brackets `<>`. For example, an opening paragraph tag looks like this: `<p>`.

Closing tags tell the browser where an element ends. They are almost the same as opening tags; the only difference is that they have a forward slash before the keyword. For example, a closing paragraph tag looks like this: `</p>`.

A full paragraph element looks like this:

```html
<p>some text content</p>
```
You can think of elements as containers for content. The opening and closing tags tell the browser what content the element contains. The browser can then use that information to determine how it should interpret and format the content.

There are some HTML elements that do not have a closing tag. These elements often look like this: `<br />` or `<img/>`, but some can also be used without the closing forward slash such as `<br>` or `<img>`. These are known as self-closing tags or empty elements because they don’t wrap any content. We will encounter a few of these in later lessons, but for the most part, elements will have both opening and closing tags.

#predefined-tags:
https://developer.mozilla.org/en-US/docs/Web/HTML/Element

"""Using the correct elements for content is called semantic HTML.
"""
HTML TAGS: that are used to structure content and define the various elements on a webpage.Each HTML tag is enclosed in angle brackets (< >) and usually consists of a start tag, content, and an optional end tag

---------------------
--------------------------

# HTML Boilerplate

### [Creating an HTML file](https://www.theodinproject.com/lessons/foundations-html-boilerplate#creating-an-html-file)

We should always name the HTML file that will contain the homepage of our websites `index.html`.

### [The DOCTYPE](https://www.theodinproject.com/lessons/foundations-html-boilerplate#the-doctype)

Every HTML page starts with a doctype declaration. The doctype’s purpose is to tell the browser what version of HTML it should use to render the document. The latest version of HTML is HTML5, and the doctype for that version is simply `<!DOCTYPE html>`.


```html
<!DOCTYPE html>
<html lang="en">
</html>
```
The **`<html>`** [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) element represents the root (top-level element) of an HTML document, so it is also referred to as the _root element_. All other elements must be descendants of this element.

#### What is the lang attribute?

`lang` specifies the language of the text content in that element. This attribute is primarily used for improving accessibility of the webpage. It allows assistive technologies, for example screen readers, to adapt according to the language and invoke correct pronunciation.

### [Head element](https://www.theodinproject.com/lessons/foundations-html-boilerplate#head-element)

The `<head>` element is where we put important meta-information **about** our webpages, and stuff required for our webpages to render correctly in the browser. Inside the `<head>`, we **should not** use any element that displays content on the webpage.

#### The charset meta element

We should always have the meta tag for the charset encoding of the webpage in the head element: `<meta charset="utf-8">`.

Setting the encoding is very important because it ensures that the webpage will display special symbols and characters from different languages correctly in the browser.

#### Title element

Another element we should always include in the head of an HTML document is the title element:

`<title>My First Webpage</title>`

The title element is used to give webpages a human-readable title which is displayed in our webpage’s browser tab.

### [Body element](https://www.theodinproject.com/lessons/foundations-html-boilerplate#body-element)

The final element needed to complete the HTML boilerplate is the `<body>` element. This is where all the content that will be displayed to users will go - the text, images, lists, links, and so on.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
  </head>

  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

--------------
-------------
# Working with Text

- How to create paragraphs
- How to create headings
- How to create bold text
- How to create italicized text
- The relationships between nested elements
- How to create HTML comments
### [Paragraphs](https://www.theodinproject.com/lessons/foundations-working-with-text#paragraphs)
If we want to create paragraphs in HTML, we need to use the paragraph element, which will add a new line after each of our paragraphs. A paragraph element is defined by wrapping text content with a `<p>` tag.

### [Headings](https://www.theodinproject.com/lessons/foundations-working-with-text#headings)
There are 6 different levels of headings starting from `<h1>` to `<h6>`. The number within a heading tag represents that heading’s level. The largest and most important heading is h1, while h6 is the tiniest heading at the lowest level.

Headings are defined much like paragraphs. For example, to create an h1 heading, we wrap our heading text in a `<h1>` tag.

### [Strong element](https://www.theodinproject.com/lessons/foundations-working-with-text#strong-element)
 you will probably find yourself using the strong element much more in combination with other text elements, like this:
```html
<p>hi welcome to the world of <strong>himalaya</strong></p>

```
The `<strong>` element makes text bold. It also semantically marks text as important; this affects tools, like screen readers, that users with visual impairments will rely on to use your website. The tone of voice on some screen readers will change to communicate the importance of the text within a strong element. To define a strong element we wrap text content in a `<strong>` tag.

### [Em element](https://www.theodinproject.com/lessons/foundations-working-with-text#em-element)

The `<em>` element makes text italic. It also semantically places emphasis on the text, which again may affect things like screen readers. To define an emphasised element we wrap text content in a `<em>` tag.

```html
<p>welcome to <em>Himalayas!</em></p>
```

### [Nesting and indentation](https://www.theodinproject.com/lessons/foundations-working-with-text#nesting-and-indentation)

You may have noticed that in all the examples in this lesson we indent any elements that are within other elements. This is known as nesting elements.

When we nest elements within other elements, we create a parent and child relationship between them. The nested elements are the children and the element they are nested within is the parent.

We use indentation to make the level of nesting clear and readable for ourselves and other developers who will work with our HTML in the future. It is recommended to indent any child elements by two spaces.

The parent, child, and sibling relationships between elements will become much more important later when we start styling our HTML with CSS and adding behaviour with JavaScript. For now, however, it is just important to know the distinction between how elements are related and the terminology used to describe their relationships.

In the following example, the body element is the parent and the paragraph is the child:
```html
<body>
	<p>page1</p>
	<p>page2</p>
</body>
```

Just as in human relationships, HTML parent elements can have many children. Elements at the same level of nesting are considered to be siblings.

For example, the two paragraphs in the following code are siblings, since they are both children of the body tag and are at the same level of nesting as each other:
```html
<body>
	<p>page1</p>
	<p>page2</p>
</body>
```

### [HTML comments](https://www.theodinproject.com/lessons/foundations-working-with-text#html-comments)

HTML comments are not visible to the browser; they allow us to _comment_ on our code so that other developers or our future selves can read them and get some context about something that might not be clear in the code.

Writing an HTML comment is simple: We just enclose the comment with `<!--` and `-->` tags. For example:
```html
<!-- this is a comment -->
<!-- this is another comment -->
```

*shortcut for HTML comments in vs code : CTRL + /*
*shortcut for creating dump text type "lorem"*

### [Additional resources](https://www.theodinproject.com/lessons/foundations-working-with-text#additional-resources)


------------------

# Lists

### [Lesson overview](https://www.theodinproject.com/lessons/foundations-lists#lesson-overview)

This section contains a general overview of topics that you will learn in this lesson.

- How to create an unordered list
- How to create an ordered list

### [Unordered lists](https://www.theodinproject.com/lessons/foundations-lists#unordered-lists)

If you want to have a list of items where the order doesn’t matter, like a shopping list of items that can be bought in any order, then you can use an unordered list.

Unordered lists are created using the `<ul>` element, and each item within the list is created using the list item element `<li>`.

Each list item in an unordered list begins with a bullet point:
```html
<ul>
	<li>chocolate</li>
	<li>biscuit</li>
	<li>waiwai </li>
</ul>

```

### [Ordered lists](https://www.theodinproject.com/lessons/foundations-lists#ordered-lists)

If you instead want to create a list of items where the order _does_ matter, like step-by-step instructions for a recipe, or your top 10 favorite TV shows, then you can use an ordered list.

Ordered lists are created using the `<ol>` element. Each individual item in them is again created using the list item element `<li>`. However, each list item in an ordered list begins with a number instead:
```html
<ol>
	<li>first class</li>
	<li>second class</li>
	<li>third class</li>
</ol>
```


# Links and Images
Links are one of the key features of HTML. They allow us to link to other HTML pages on the web. In fact, this is why it’s called the web.

- How to create links to pages on other websites on the internet
- How to create links to other pages on your own websites
- The difference between absolute and relative links
- How to display an image on a webpage using HTML

### [Anchor elements](https://www.theodinproject.com/lessons/foundations-links-and-images#anchor-elements)

To create a link in HTML, we use the anchor element. An anchor element is defined by wrapping the text or another HTML element we want to be a link with an `<a>` tag.

Add the following to the body of the index.html page we created and open it in the browser:

```html
<a>click me</a>
```
You may have noticed that clicking this link doesn’t do anything. This is because an anchor tag on its own won’t know where we want to link to. We have to tell it a destination to go to. We do this by using an HTML attribute.

An HTML attribute gives additional information to an HTML element and always goes in the element’s opening tag. An attribute is usually made up of two parts: a name, and a value; however, not all attributes require a value. In our case, we need to add a href (hyperlink reference) attribute to the opening anchor tag. The value of the href attribute is the destination we want our link to go to.

Add the following href attribute to the anchor element we created previously and try clicking it again, don’t forget to refresh the browser so the new changes can be applied.

```html
<a href="https://www.theodinproject.com/about">click me</a
```

By default, any text wrapped with an anchor tag without a `href` attribute will look like plain text. If the `href` attribute is present, the browser will give the text a blue color and underline it to signify it is a link.

It’s worth noting you can use anchor tags to link to any kind of resource on the internet, not just other HTML documents. You can link to videos, pdf files, images, and so on, but for the most part, you will be linking to other HTML documents.

### [Opening links in a new tab](https://www.theodinproject.com/lessons/foundations-links-and-images#opening-links-in-a-new-tab)

The method shown above opens links in the same tab as the webpage containing them. This is the default behaviour of most browsers and it can be changed relatively easily. All we need is another attribute: the `target` attribute.

While `href` specifies the destination link, `target` specifies where the linked resource will be opened. If it is not present, then, by default, it will take on the `_self` value which opens the link in the current tab. To open the link in a new tab or window (depends on browser settings) you can set it to `_blank` as follows:
```html
<a href="https://www.theodinproject.com/about" target="_blank" rel="noopener noreferrer">click me</a>
```

You may have noticed that we snuck in the `rel` attribute above. This attribute is used to describe the relation between the current page and the linked document.

The `noopener` value prevents the opened link from gaining access to the webpage from which it was opened. The `noreferrer` value prevents the opened link from knowing which webpage or resource has a link (or ‘reference’) to it. It also includes the `noopener` behaviour and thus can be used by itself as well.

*security concerns*
Why do we need this added behaviour for opening links in new tabs? Security reasons. The prevention of access that is caused by `noopener` prevents [phishing attacks](https://www.ibm.com/topics/phishing) where the opened link may change the original webpage to a different one to trick users. This is referred to as [tabnabbing](https://owasp.org/www-community/attacks/Reverse_Tabnabbing). Adding the `noreferrer` value can be done if you wish to not let the opened link know that your webpage links to it.

Note that you may be fine if you forget to add `rel="noopener noreferrer"` since more recent versions of browsers [provide this security](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#security_and_privacy) if only `target="_blank"` is present. Nevertheless, in line with good coding practices and to err on the side of caution, it is recommended to always pair a `target="_blank"` with a `rel="noopener noreferrer"`.

### [Absolute and relative links](https://www.theodinproject.com/lessons/foundations-links-and-images#absolute-and-relative-links)

Generally, there are two kinds of links we will create:

1. Links to pages on other websites on the internet
2. Links to pages located on our own websites

#### Absolute links

Links to pages on other websites on the internet are called absolute links. A typical absolute link will be made up of the following parts: `protocol://domain/path`. An absolute link will always contain the protocol and domain of the destination.

We’ve already seen an absolute link in action. The link we created to The Odin Project’s About page earlier was an absolute link as it contains the protocol and domain.

`https://www.theodinproject.com/about`

#### Relative links

Links to other pages within our own website are called relative links. Relative links do not include the domain name, since it is another page on the same site, it assumes the domain name will be the same as the page we created the link on.

Relative links only include the file path to the other page, _relative_ to the page you are creating the link on. This is quite abstract, let’s see this in action using an example.

Within the `odin-links-and-images` directory, create another HTML file named `about.html` and paste the following code into it:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Odin Links and Images</title>
  </head>

  <body>
    <h1>About Page</h1>
  </body>
</html>
```

Back in the index page, add the following anchor element to create a link to the about page:

```html
<body>
  <h1>Homepage</h1>
	<a href="https://www.theodinproject.com/about">click me</a>

	<a href="about.html">About</a>
</body>
```

Open the index file in a browser and click on the about link to make sure it is all wired together correctly. Clicking the link should go to the about page we just created.

This works because the index and about page are in the same directory. That means we can simply use its name (`about.html`) as the link’s href value.

But we will usually want to organize our website directories a little better. Normally we would only have the index.html at the root directory and all other HTML files in their own directory.

Create a directory named `pages` within the `odin-links-and-images` directory and move the `about.html` file into this new directory.

Refresh the index page in the browser and then click on the about link. It will now be broken. This is because the location of the about page file has changed.

To fix this, we just need to update the about link href value to include the `pages/` directory since that is the new location of the about file _relative_ to the index file.

```html
<body>
  <h1>Homepage</h1>
  <a href="pages/about.html">About</a>
</body>
```

Refresh the index page in the browser and try clicking the about link again, it should now be back in working order.

In many cases, this will work just fine; however, you can still run into unexpected issues with this approach. Prepending `./` before the link will in most cases prevent such issues. By adding `./` you are specifying to your code that it should start looking for the file/directory _relative_ to the `current` directory.

```html
<body>
  <h1>Homepage</h1>
  <a href="./pages/about.html">About</a>
</body>
```

#### A metaphor

Absolute and relative links are a tricky concept to build a good mental model of, a metaphor may help:

Think of your domain name (`town.com`) as a town, the directory in which your website is located (`/museum`) as a museum, and each page on your website as a room in the museum (`/museum/movie_room.html` and `/museum/shops/coffee_shop.html`). Relative links like `./shops/coffee_shop.html` are directions from the current room (the museum movie room `/museum/movie_room.html`) to another room (the museum shop). Absolute links, on the other hand, are full directions including the protocol (`https`), domain name (`town.com`) and the path from that domain name (`/museum/shops/coffee_shop.html`): `https://town.com/museum/shops/coffee_shop.html`.


### [Images](https://www.theodinproject.com/lessons/foundations-links-and-images#images)

Websites would be fairly boring if they could only display text. Luckily HTML provides a wide variety of elements for displaying all sorts of different media. The most widely used of these is the image element.

To display an image in HTML we use the `<img>` element. Unlike the other elements we have encountered, the `<img>` element is self-closing. Empty, self-closing HTML elements do not need a closing tag.

Instead of wrapping content with an opening and closing tag, it embeds an image into the page using a src attribute which tells the browser where the image file is located. The src attribute works much like the href attribute for anchor tags. It can embed an image using both absolute and relative paths.

For example, using an absolute path we can display an image located on The Odin Project site:

To use images that we have on our own websites, we can use a relative path.

1. Create a new directory named `images` within the `odin-links-and-images` project.
    
2. Next, download [this image](https://unsplash.com/photos/Mv9hjnEUHR4/download?force=true&w=640) and move it into the images directory we just created.
    
3. Rename the image to `dog.jpg`.
    

Finally add the image to the `index.html` file:
```html
<body>
  <h1>Homepage</h1>
	<a href="https://www.theodinproject.com/about">click me</a>

	<a href="./pages/about.html">About</a>

	<img src="./images/dog.jpg">
</body>
```

### [Parent directories](https://www.theodinproject.com/lessons/foundations-links-and-images#parent-directories)

What if we want to use the dog image in the about page? We would first have to go up one level out of the pages directory into its parent directory so we could then access the images directory.

To go to the parent directory we need to use two dots in the relative filepath like this: `../`. Let’s see this in action, within the body of the `about.html` file, add the following image below the heading we added earlier:
```html
<img src="../images/dog.jpg">
```
To break this down:

1. First, we are going to the parent directory of the pages directory which is `odin-links-and-images`.
2. Then, from the parent directory, we can go into the `images` directory.
3. Finally, we can access the `dog.jpg` file.

Using the metaphor we used earlier, using `../` in a filepath is kind of like stepping out from the room you are currently in to the main hallway so you can go to another room.
### [Alt attribute](https://www.theodinproject.com/lessons/foundations-links-and-images#alt-attribute)

Besides the src attribute, every image element must also have an alt (alternative text) attribute.

The alt attribute is used to describe an image. It will be used in place of the image if it cannot be loaded. It is also used with screen readers to describe what the image is to visually impaired users.

This is how the The Odin Project logo example we used earlier looks with an alt attribute included: