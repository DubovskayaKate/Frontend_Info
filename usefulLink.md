[Modal window](https://nisnom.com/modalnye-okna/)

[Dynamic window](http://dbmast.ru/vydvigayushheesya-bokovoe-menyu-na-chistom-css)

[Dynamic window](http://gnatkovsky.com.ua/poyavlenie-bloka-pri-navedenii-s-pomoshhyu-css.html)

[Dropdown menu](https://www.w3schools.com/css/css_dropdowns.asp)

[Navigation](https://www.w3schools.com/css/css_navbar.asp)

[FlexBox](https://html5.by/blog/flexbox/)


[CSSComp](https://csscomb.herokuapp.com/online)

### [CSS Selectors](https://learn.shayhowe.com/advanced-html-css/complex-selectors/)


|Example|Classification|Explanation|
|---|---|---|
|h1|Type Selector|Selects an element by its type|
|.tagline|Class Selector|Selects an element by the class attribute value, which may be reused multiple times per page|
|#intro|ID Selector|	Selects an element by the ID attribute value, which is unique and to only be used once per page|
|article|h2|Descendant Selector	Selects an element that resides anywhere within an identified ancestor element|
|article > p|Direct Child Selector|Selects an element that resides immediately inside an identified parent element|
|h2 ~ p|General Sibling Selector|Selects an element that follows anywhere after the prior element, in which both elements share the same parent|
|h2 + p|Adjacent Sibling Selector|Selects an element that follows directly after the prior element, in which both elements share the same parent|
|a[target]|	Attribute Present Selector|	Selects an element if the given attribute is present|
|`a[href="http://google.com/"]`|Attribute Equals Selector|Selects an element if the given attribute value exactly matches the value stated|
|a[href*="login"]|Attribute Contains Selector|Selects an element if the given attribute value contains at least once instance of the value stated|
|a[href^="https://"]|Attribute Begins With Selector|Selects an element if the given attribute value begins with the value stated|
|a[href$=".pdf"]|Attribute Ends With Selector|Selects an element if the given attribute value ends with the value stated|


### Pseudo-classes

||||
|---|---|---|
|a:link	Link| Pseudo-class|Selects a link that has not been visited by a user|
|a:visited|	Link Pseudo-class|Selects a link that has been visited by a user|
|a:hover|	Action Pseudo-class|Selects an element when a user has hovered their cursor over it|
|a:active|	Action Pseudo-class|Selects an element when a user has engaged it|
|a:focus|Action Pseudo-class|Selects an element when a user has made it their focus point|
|li:first-child|Structural Pseudo-class|Selects an element that is the first within a parent|
|li:last-child|	Structural Pseudo-class|Selects an element that is the last within a parent|


### Pseudo-element

||||
|---|---|---|
|.alpha:first-letter|Textual| Pseudo-elements|Selects the first letter of text within an element|
|.bravo:first-line|	Textual|Pseudo-elements	Selects the first line of text within an element|
|div:before|Generated Content|Creates a pseudo-element inside the selected element at the beginning|
|a:after|Generated Content|Creates a pseudo-element inside the selected element at the end|
|::selection|Fragment Pseudo-element|Selects the part of a document which has been selected, or highlighted, by a users’ actions|