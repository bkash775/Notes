
### [Introduction](https://www.theodinproject.com/lessons/foundations-the-box-model#introduction)

Now that you understand the basic syntax of HTML and CSS, we’re going to get serious. The most important skills you need to master with CSS are _positioning_ and _layout_. Changing fonts and colors is a crucial skill, but being able to put things exactly where you want them on a webpage is even more crucial. After all, how many webpages can you find where absolutely every element is just stacked one on top of another?

Learning to position elements on a webpage is not that difficult once you understand just a few key concepts. Unfortunately, many learners race through learning HTML and CSS to get to JavaScript and end up missing these fundamental concepts. This leads to frustration, pain, ([and funny gifs](https://giphy.com/gifs/css-13FrpeVH09Zrb2)) because all the JavaScript skills in the world are meaningless if you can’t stick your elements on the page where you need them to be. So with that in mind, let’s get started.

### [Lesson overview](https://www.theodinproject.com/lessons/foundations-the-box-model#lesson-overview)

This section contains a general overview of topics that you will learn in this lesson.

- You’ll learn all about _the box model_.
- You’ll learn how to make sure elements are just the right size with `margin`, `padding`, and `borders`

### [The box model](https://www.theodinproject.com/lessons/foundations-the-box-model#the-box-model)

The first important concept that you need to understand to be successful in CSS is the box model. It isn’t complicated, but skipping over it now will cause you much frustration down the line.

Every single thing on a webpage is a rectangular box. These boxes can have other boxes in them and can sit alongside one another. You can get a rough idea of how this works by sticking a border on every item on the page like this:

```css
* {
  border: 2px solid red;
}
```

[![boxes](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/boxes.png)](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/boxes.png)

You can use the browser’s inspector to add the CSS above to this web page if you want. Boxes in boxes!

[![lines](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/odin-lined.png)](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/odin-lined.png)

OK, so there might be some circles in the above image… but when it comes to layout, they fit together like rectangular boxes and not circles. In the end, laying out a webpage and positioning all its elements is deciding how you are going to nest and stack these boxes.

The only real complication here is that there are many ways to manipulate the size of these boxes, and the space between them, using `padding`, `margin`, and `border`. The assigned articles go into more depth on this concept, but to sum it up briefly:

- `padding` increases the space between the border of a box and the content of the box.
- `margin` increases the space between the borders of a box and the borders of adjacent boxes.
- `border` adds space (even if it’s only a pixel or two) between the margin and the padding.

Be sure to study the diagrams carefully.

[![the box model](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/box-model.png)](https://cdn.statically.io/gh/TheOdinProject/curriculum/main/foundations/html_css/css-foundations/the-box-model/imgs/box-model.png)

