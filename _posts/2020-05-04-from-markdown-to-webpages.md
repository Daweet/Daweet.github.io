# From Markdown to Website pages
I am going to use Markdown to create my webpages, the question is how do you achieve that? So how do you create the web pages? 
This is actually straight forward. You see, once you put some content in Markowndown files you then need to commit to your 
repository, following which GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, this is done 
from the content in your Markdown files.

This implies that it is important to have a look at how to use Markdown. That is what we see in this post.

## Markdown
What’s Markdown?
Here is a definition provided in the website of [Markdown](https://www.markdownguide.org).
> Markdown is a lightweight markup language that you can use to add formatting elements to plaintext text documents. Created
> by John Gruber in 2004, Markdown is now one of the world’s most popular markup languages.
> Using Markdown is different than using a WYSIWYG editor. In an application like Microsoft Word, you click buttons to 
>  format words and phrases, and the changes are visible immediately. Markdown isn’t like that. When you create a Markdown-
>   formatted file, you add Markdown syntax to the text to indicate which words and phrases should look different.
> For instance, to denote a heading, you add a number sign before it (e.g., # Heading One). Or to make a phrase bold, you 
> add two asterisks before and after it (e.g., **this text is bold**). It may take a while to get used to seeing Markdown 
> syntax in your text, especially if you’re accustomed to WYSIWYG applications.

I think Markdown is intuitive, a lightweight and easy-to-use syntax for styling your writing. You can write simple blogs, equations, codes with it. 
You can also share other sites and add images to your blogs and/or files. I suggest to visit the website [Markdown](https://www.markdownguide.org) 
for further exploration.

I came across Markdown for the first time when I joined Data Work Academy bootcamp provided by [Montgomery](https://www.montgomerycollege.edu).
I used it for the first time to put description on [Jupyternotebook](https://jupyter.org) for python codes that I was working on. Honeslty speakiong
I was mesmerised by how easy it is to write down, and amazed by its simpicity. Of course you need to follows syntax to write down,yet the syntax are simple and intuitive enough to grasp.

### Headings
Accordingly Markdown follows some syntax conventions.
To begin with the syntax to make a heading is the number sign (#), the size varies depending how many # you put going from one to six, one being the biggest and six the smalles size for heading, as shown below

```
# Chapter 1
## Section 1.1
### Subsection 1.1.1
#### Sub-Subsection 1.1.1.1
##### Sub-Sub-Subsection 1.1.1.1.1
###### Sub-Sub-Sub-Subsection 1.1.1.1.1.1.
```
This provides the following output
# Chapter 1
## Section 1.1
### Subsection 1.1.1
#### Sub-Subsection 1.1.1.1
##### Sub-Sub-Subsection 1.1.1.1.1
###### Sub-Sub-Sub-Subsection 1.1.1.1.1.1

### Emphasis
You can emphasize what you want using the fowllowing syntax, for comperhensive syntax and usage visit [Adam Pritchard ](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
```
Emphasis can be done using, *asterisks* or _underscores_.

Strong emphasis, i.e bold, with **asterisks** or __underscores__.

You can also combine emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```
Emphasis can be done using, *asterisks* or _underscores_.

Strong emphasis, i.e bold, with **asterisks** or __underscores__.

You can also combine emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

### Lists, ordered and unordered

To create an ordered list, add line items with numbers followed by periods. The numbers don’t have to be in numerical order, but the list should start with the number one.
```
Items to purchase put in ordered list
1. Apple
2. Banana
3. Cantaloupe
4. ..
```
This yields the following orderd list 
1. Apple
2. Banana
3. Cantaloupe
4. ...
If you want to create unordered list, you type either _, +, or * and space followed by what you want to write
```
Unordered can be written as 
- Apple
  + 1 pound
- Banana
  * 2 pound
- Cantaloupe
```

Resulting the following unorderdd list along with thier sublistsz
- Apple
  + 1 pound
- Banana
  * 2 pound
- Cantaloupe


**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Daweet/Daweet.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.
