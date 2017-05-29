For a lot of bug bounty platforms, code repos and chat clients, Markdown is used as the notation to outline whatever you're writing up. However having read many reports on various platforms, it seems folks either are too lazy to use it or just don't know. Here's a quick 101 on how to do markdown! 

## Starting Simple

Markdown is essentially a shorthand way to manage post formatting and styling, it shares a lot of similarities with HTML. Like in a HTML document you can have headers with `<h1></h1>` this works in markdown as shown:

<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>

However you can also just use the `#` character to notate headings:

```
#Heading 1
##Heading 2
###Heading 3
####Heading 4
#####Heading 5
######Heading 6
```
#Heading 1
##Heading 2
###Heading 3
####Heading 4
#####Heading 5
######Heading 6

Writing in Markdown is really easy. Simply write as you normally would however Where appropriate, you can use *shortcuts* to **style** your content. italics are noted with `*text*` and bold with `**bold**` .

For example, a list would look like this in raw markdown.
```
* Item number one
* Item number two
    * A nested item
* A final item
```
When rendered it will look like this:

* Item number one
* Item number two
    * A nested item
* A final item

You can also do the same with numbers:
```
1. Numbered list 1
2. Numbered list 2
3. Numbered list 3
```

1. Numbered list 1
2. Numbered list 2
3. Numbered list 3

### Links

Want to link to a source? No problem. If you paste in a URL, like https://blog.zsec.uk - in some implementations of markdown it will automatically be linked up. Alternatively as a gold standard you can link to thinks like this: `[Follow ZephrFish](https://twitter.com/ZephrFish)` which looks like this when it's rendered [Follow ZephrFish](https://twitter.com/ZephrFish). Essentially the structure is `[whatever you want your link to be](link)`.

### Embedding Images

Images work too! Already know the URL of the image you want to include in your report/document? Simply paste it in like this to make it show up:

`![ZeroSec Logo](https://blog.zsec.uk/content/images/2016/07/logo-2.png)`
![ZeroSec Logo](https://blog.zsec.uk/content/images/2016/07/logo-2.png)

Not sure which image you want to use yet? That's cool too. Leave yourself a descriptive place holder and keep writing. Come back later and simply click on the box to upload. Note this isn't the same for some platforms such as [Hackerone](https://hackerone.com) as they used curly braces to embed images upon upload to a report. Simply put when you up load an image to a report it gets a number assigned which you have to reference like `{F1337}` this will change per report.

`![uber 0day]`
![uber 0day]

## Adding Dividers

Throw 3 or more dashes(`---`) down on any new line and you've got yourself a fancy new divider.] Funky!

---

See, like this...

---


### Quoting Things

Sometimes a link isn't enough when referencing things, you want to quote someone or a website on what they've/it has said. Maybe you are quoting part of an article or a slogan.

`> ZeroSec - The InfoSec Ninjas`
> ZeroSec - The InfoSec Ninjas

### Working with Code Blocks

The beauty of markdown is the ability to write in-line code and be able to show it. This is notated with a pair back ticks: ` `` `. If a single line isn't enough, maybe you're showing an example request and response this can also be achieved with three back ticks like so(without the `'`) ' ``` ':

```
GET / HTTP/1.1
Host: blog.zsec.uk
Connection: close

```

The above block shows a GET request to `blog.zsec.uk` in a block of text. Using codeblocks doesn't need to be just for code however, it can also be used instead of a quote block if applicable. 


###  More HTML Usage

If you don't like the funkiness of markdown you can just write plain old HTML and it'll still work! Very flexible.

`<input type="text" placeholder="Input Stuff?" />`

<input type="text" placeholder="Input Stuff?" />


This guide should be enough to get you started writing some pretty markdown however there are many guides out there on more advanced usage.

Did you enjoy this? Check out the other #ltr101 posts [here](https://blog.zsec.uk/tag/ltr101/) or consider buying my [book](https://leanpub.com/ltr101-breaking-into-infosec).
