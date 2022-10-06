<h1 align="center">
CSS Guide
</h1>

<h2 align="center"> CSS Getting Started </h2>

- CSS3 - is currently in development (In Development)
- THERE WILL NEVER BE CSS 4 - because 3 is being split into modules: (colors, animation)
- SCHEDULE:	
	- Basic: 		Sec 2:Basics - Sec 6: Positioning
	- Advanced: 	Sec 7:Backrounds and images - Sec 12:Text and Fonts
	- Expert:		Sec 13:Flexbox - Sec 18:Sass

<h2 align="center"> Diving into the Basics of CSS </h2>

### Inline Styles
```html		
<someTag style="property: value;">
```

### \<style\> Tags
```html
<style>
	selector {
		property: value;
	}
</style>
```

### .css Files			
- You can put your CSS rules in a file like main.css
- Link the css to html with:
	```html
	<link rel="stylesheet" href="somePath"> //no closing tag is required
	```

### Properties and Values
``` css
backround: #345677			//Background Color
color: #123456 				//Text Color
font-family: sans-serif 	//Changes the font family
```		
- You can use Google Fonts > + > Import Link above our style link > Add rule
	
### Selectors		
- Element Selectors
	```css
	someTagName{props:vals;}
	```
	- Applied to all tags
- Class Selectors
	```css 
	.someClassName{props:vals;}
	```
	- Applied to tags with `class=""` attributes
- Universal Selectors
	```css
	* {props:vals;}
	```
	- Applied to every element in the page
- ID Selectors
	```css
	#someIDName {props:vals;}
	```
	- Applied to one element with `id=""` attribute
- Attribute Selectors
	```css
	[someAttributeName] {props:vals;}
	```
	- Applied to elements based on their attributes

### Cascading Style (specificity)	
- Inline styles have the highest priority
- If two rules manipulate the same property. The one with higher specificity wins (ex: class selectors beats element selectors)
- If there are multiple rules with the same selector. The last on in the file is applied
- Specificity Rankings:(Descending from high to low priority)
	1. Inline Styles
	2. #ID selectors
	3. .class, :pseudo-class and [attribute] selectors
	4. \<tag\> and pseudo-element selectors
	5. *Universal selector
	6. Browser provided styles

### Inheritance			
- We can propagate down a rule simply by styling a parent that has child elements.
- Although, if a child defines their own rule then the specificity is better for that child and overrides the parent's
- Having a rule in the body tag is perfect for propagating rules to every element

### Combinators			
- Allow us to combine multiple selectors to be more precise about what we select
	- Ex:	`#someID h1 {}`	=	"Any h1 tag inside of #someID
	- This increases specificity
	- FOUR IMPORTANT TYPES OF COMBINATORS (examples use two, but can be more)
		1. Adjacent Sibling:
			- Notation: `+`
			- Ex:	`h2 + p`	=  "Assigns to all paragraphs that directly follow an h2 tag"
		2. General Sibling:
			- Notation: `~`
			- Ex:	`h2 ~ p`	=  "Assigns to all paragraphs that have a sibling h2 anywhere; order doesn't matter."
		3. Child:
			- Notation: `>`
			- Ex:	`div > p`	=  "Any paragraph that's a DIRECT child of a div should get the style"
		4. Descendant
			- Notation: \<whitespace\>
			- Ex:	`div p` =    "Any paragraph that's a descendant of a div should get a style" (Direct not needed)

<h2 align="center"> DIVING DEEPER INTO CSS </h2>

### CSS Box Model			
- In the webpage, HTML interprets elements as boxes (Check inspector below properties for visuals)
- Every element box has content, padding, border, and margins
- CSS properties: 
	- `padding: 20px;` 
	- `border: 5px black solid;` (this is shorthand)

### Default Margins		
- By default, the \<body\> element has a default margin of 8px. We can remove this with margin: 0px;

### Margin Collapsing		
- The bigger margin of two adjacent box elements is the things that separates both from each other, therefore the margins don't add up, they "collapse" onto each other.

### Shorthand Properties		
- Combine values of multiple other properties, into a single property. For example -
	```css
	/* Separate properties: */
				border-width: 2px;
				border-style: dashed (or solid);
				border-color: orange;
	/* Shorthand property: */
				border: 2px solid orange;
	```
	- The order doesn't matter as long as each property uses a different value.
	- margin has a shorthand for its margin-top , right, bottom, left properties like so:
		```css
		margin: 5px, 10px, 5px, 10px; /* (in the order above) (there's a 2 side version using only two Vals for top/down) */
		```

### Height & Width properties	
- Can have relative values (%) or absolute values (px)
- Width can take up the full width of the page. 
- Height can not take up the full height of a page. 100% refers to the available height given by the parent's container.
	- Quirky. 100% needs to be chained down from <html> in order to get it to work like width's 100%
	- Better to use absolute values.
- The width and height by default manipulates only the content section our CSS box. Therefore if you add padding, borders and margins. IT maybe glitchy at 100%. THIS IS BECAUSE THE PROPERTY box-sizing IS SET TO content-box by default. You could set for everything before the margin section to be included in width and height calculations by setting the value `border-box`. **THIS VALUE IS SO COMMON BECAUSE THAT IS THE EFFECT WE WANT TO KEEP EVERYTHING BALANCED. ILL' REPEAT** -						
	- **IMPORTANT PROPERTY** (from above) - `box-sizing: border-box;`
	- This is something you should wrap in a universal selector for all elements to use.

### Display Property		
- Allows us to change the behavior of an element from block to inline (bidirectional) and even remove the element from the DOM
	- Inline elements don't take the full width of the screen, and don't have margin top/bottom. Blocks do.
- The property is `display:` and has the values -
	- block 
		- makes an element into a block
	* inline
	* none	
		- makes an element invisible in the DOM but doesn't remove it from the DOM
		- `visibility: hidden;` is similar but it blocks content from taking its place
	- inline-block (keep the behavior of blocks, but they can go next to each other now)
		- WEIRD QUIRK: HTML reads whitespace as a node, so if you try to make two inline-blocks stay in the same line using 100% and minus, you have to take into account that whitespace node's size
- THE DIFFERENCE BETWEEN INLINE AND BOX:
	- Box take up the whole width of a document
	- Inline takes up the space that is needed for its content
		- So we'd have to be careful in cases like:
			1. Using width 100% would make a new line away from another inline. We would have to do width: 100% - someOtherInlinesWidth; to make it stay in the same line
- Later on we see flex box which is better.
					
### Calc()				
- To yield a calculate value do something like:
	```css
	width: calc(100% - 53px) /*Here whitespace between each operand matters*/
	```
### text-decoration and vertical-align		
- For things with default styling like anchor tags. We can do `text-decoration: none;` to get rid of its default styling
- The font size of one thing in the "line" can effect the alignment of the others. (Ex: our logo is big and our Nav links align to "touch the floor" with the big logo). We can feed the value `vertical-align: middle;` to remedy quirky alignment

### Pseudo Classes			
```css
someSelector:pseudoclass {//rules}
```
- **DEFINITION** - Defines the style of a special state of an element
- **EXAMPLE** -
	- `someSelector:hover{}` - when your mouse hovers over the selected element then a rule is applied while it stays there.

### Pseudo Elements		
```css
someSelector::pseudoclass {/rules}
```
- DEFINITION - Define the style of a specific part of an element
- EXAMPLE -
	- `someSelector::after{content: "(Link)"}` - Puts the text in content after the selected element

### Grouping Rules			
- Simply comma separate selectors. And the rules will be applied to all

### border-bottom			
- Value -  `5px solid white;` - to add a white line below a link
	
### Tags with multiple class=""		
- This can be done by adding another className to the attribute after a space

### Adding an image
```css
background: url("freedom.jpg");
```

### Properties worth remembering
- color
- background-color
- display
- padding
- border
- margin
- width
- height


<h2 align="center"> MORE ON SELECTORS & CSS FEATURES </h2>
### Tags can have more than one class in the class attribute (separated by white space)
 - We can make separate rules for each class. Though if they manipulate the same thing order matters (bottom-most in CSS file)

### Select by Class		
```css
tagType.className {}  /*Targets an element which is of tagType and has the className CSS class*/
```

### Overwrite specificity using !important
```css
div { color: red !important;}	/*You SHOULD NOT USE this. Leads to bad code*/
```

### Selecting the opposite with :not() pseudo class
```css
a:not(someSelector) {}	/*Select any anchor tag that does not have class="someSelector"*/
```

<h2 align="center"> PRACTICING THE BASICS </h2>

### text-align property		
- To align the text to the center (width-wise) the value should be: center
	
### box-shadow property
```css		
box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.5);	/*Yields a "box shadow" on your selection, NOTICE the COLOR FUNCTION rgba()
```

### border-radius property
```css	
border-radius: 8px; /*for all corners*/		
border-radius: 4px 4px 4px 4px /*To define every corner*/
```

### Cursor when we hover over buttons
```css
cursor: pointer;
```		
	
### For buttons use Pseudoclasses			
- `:hover` and `:active` can change the color to make it more pleasant

### Getting rid of the default blue outline when focusing on buttons
```css
.button:focus{ outline: none;}
```

### Vertical align		
- Use it on the parent of things you want to vertically align

- Turning a block into a perfect circle			
```css
border-radius: 50%;
width: 128px; 
height: 128px;	/*Where both w/h need to be the same*/
```

### To center things horizontally		
```css
margin: auto;	/*which centers on all directions (mostly horizontally)*/
```

### "floats"			
- **Definition** - Overwrite default positioning and tell the browser that an element should go to the left or right (isn't used much now because we have flexbox now)
	- **Warning** -	It takes the element out of the document flow. (other elements can overlap under it). To prevent this we define an empty div behind this element that has the rule `clear: both;`

<h2 align="center"> POSITIONING ELEMENTS WITH CSS </h2>

### Positioning Theory		
- The position property is applied by default with the value static
- position can hold the values:	`static`, `absolute`, `relative`, `fixed`, `sticky`		//"move to another position."
- But it must also define where it should be placed so we have the properties -		//"ok, but where should I move to?"
	- `left`, `right`, `bottom`, `left` (or combos of these)
	- When top is defined with a value of say 20 px It can have two meanings:  //positioning context is always the viewport
		1. "Move the current element 20px above where it currently is."
		2. "Move the current element 20px from the top of some element"
- top, left, etc takes effect on when position is anything but static

### Working with "fixed"		
- When you first define `position: fixed;` you'll see that elements below the "fixed" overtake it and it's width acts like an inline-block. This happens because we took it out of the "document flow" (this element doesn't exist to the others)
	- When you define "top: 0px" (and make sure your margin is 0): WE SEE THAT IT IS FIXED RELATIVE TO THE BROWSER VIEWPORT.
	- So if you define a property left & top with 0 values, then it is "fixed" starting at the top left!

### "position" to add a background image		
- Properties:
	- background: url("somePathToImage");
	- position: fixed;
	- width & height: 100%;

### Understanding the "z-axis" property		
- By default every element has a z-property with the value auto (which is equal to 0)
	- So to stack things above. Choose any number greater than what you want to stack above. And below, less than (can be negative)
		- two values w/ same Z rely on the order of the HTML to appear
	- **IMPORTANT** - z-index only works with elements that have the position property with any value except for static.

### Adding a badge to our Package (absolute)		
- The `absolute` property. The positioning context is defined by TWO CASES:
	1. If none of the ancestors of the element has a position property define. Then the positioning context of our element is the \<html\> element
	2) If not (1) then the closest ancestor w/ a position is the positioning context for this element
- absolute also breaks the "flow"

### Styling & Positioning w/ absolute & relative 
- Use relative to not "break" the flow and in order to let an absolute have it's position relative to that relative.
	 		* We could have our top, left, right, bottom to be equal to 0 and let margins define the distance away.

### Diving Deeper into relative	
- DOESN'T BREAK FLOW
- IDEAL TO SETUP POSITIONING CONTEXT's FOR OTHER ELEMENT's that use absolute.
- When top, left, right, bottom are used on elements that have the property RELATIVE. Then it move's offset to it's "flow" position. AND WHEN YOU DO THIS IT STILL ISN'T TAKEN OUT OF ITS FLOW!!!

### Working w/ "overflow" & relative positioning (to from leaving a parent)		
- On the rule w/ the parent selected add:		overflow: hidden;
- A WEIRD QUIRK OF HTML: if `overflow: hidden;` is passed to a \<body>, it erases it and passes it to the \<html> to prevent offsets to a child		
	- **FIX** - Add the overflow TO BOTH \<body> & \<html>
	 
### position: sticky;		
* Does nothing alone. If something like `top: 20px` is defined, then once the viewport top reaches the element w/ sticky by 20px, it will follow it until it reaches the end of the parent

### Understanding the Stacking Context: 		
- If all elements are fixed and are in the same Z. Then, the html code dictates how they stack (in order seen)
- Children can not go behind the parent's Z-index AND it can't go above elements that are not related to it. This is because elements that have fixed on them define their own STACKING CONTEXT

<h2 align="center"> UNDERSTANDING BACKGROUND IMAGES & IMAGES </h2>

- There's more to background that we've seen. When we define an image, we actually set the background-image. With color it's background-color.
- There can be multiple images in a background property. Simply comma seperate them in the short hand with as many other properties as you want. This works with transparency

### background-size		
* If you set `100px`  you set the w&h, two args means width then height.
* You can do ^ with percentages
* To keep the aspect ratio: you can define one arg w/ a value and then
	use auto for the other.
* Can also have the value: `cover`; which basically is similar to 100% except it would
	even zoom in if the image is smaller than the container
* The value: `contain;` ensures that the full image is visible in the container
	it doesn't care that whitespace appears

### background-repeat		
- `no-repeat;` so the image isn't repeated
* `repeat-x;` to only repeat on the x-axis (there's a y version too)

### background-position	
- pixel or percent values
- left most arg pushes the left side of the background, next arg pushes from the top
- **IMPORTANT** - When you use percentages it actually defines shifting of "the hidden parts of the image" not the whole image itself
- **EX** - So if we do `background-position: 0px 100%;`. We are saying "All the content that has to be cropped at the top will be cropped at the top, meaning all of the bottom since nothing is cropped there.
- **EASY VALUES** -
	- `center;` //50% is cropped out from all directions
	- `left top;` 
		- left edge of the image is positioned on the left edge of container
		- same for top, SO NO CROPPING HAPPENS ON LEFT & TOP
	- THESE KEY WORDS CAN STILL BE COMBINED W/ PERCENTS

### background shorthand	
- Properties that affect images
	- image : set one or more background images
	- position : set init position, relative to background position layer
	- size : 
	- repeat : 
	- origin : set bg positioning area
	- attachment : set the scrolling behaviour of bg image
- Properties that don't
	- color
	- clip : defines whether backgrounds extend underneath borders

### background clip, origin, attachment		
- WORK EXACTLY LIKE BOX-SIZING
- origin & clip CAN WORK TOGETHER:
	- They take values: `border-box`, `padding-box`, `content-box`
	- origin may use border-box to have the image go through the border
	- origin's padding-box value would not get rid of excess image that isn't cropper, therefore for background-clip we define padding-box to get rid the image up to the padding section. **THIS YIELDS AN IMAGE THAT IS PERFECTLY INSIDE THE BOX W/ BORDERS BUT NOT UNDERNEATH THE BORDER ITSELF**
- attachment: rarely used but is used w/ scrolling effects

### background shorthand for image, pos, size	
```css
background: image position/size repeat (origin clip) attachment;
									/*1 for same ^ 2 for diff*/
```

### Styling images (not backgrounds)		
- When setting sizes, your selector should directly reference the \<img>, else, the  \<img> will use it's default size
- height: some% is relative to the \<img>'s default size. Not the parent node.
- The parent element of the \<img> should be inline-block to respect sizing

### Linear & Radial Gradients		
- Gradients are treated as images so it's property is background-image
- Value Examples -
	- `linear-gradient( to left bottom, red, blue);` 
		- First arg is the direction. Can be word syntax or degrees
		- You can also define as many colors as needed. Useful color is transparent
		- Put percents at the right of colors to define where the gradient starts
	- `radial-gradient`( circle at top (or 20% 50%), red, blue);

### filter				
- Values -
	- `blur(px);`
	- `grayscale(%);`

<h2 align="center"> SIZES & UNITS </h2>

	-Units: 				* root em (rem): 				a unit that refers to font size
							* em (em): 						also refers to the font size
							* viewport height (vh):
							* viewport width (vw):

	-Categories of			* Absolute Length:
	 Units:							** Avoid using  any absolute length other than pixel (so don't use things like cm or mm because it actually dependes on the pixel count of a screen)
	 						* Viewport Lenths:
									** Adjust sizes of elements according to viewport
									** Units: vh, vw, vmin, vmax
							* Font-Relative Lengths:
									** Adjust to the default font size.
									** Units: rem, em, etc...
							* Fixed Lengths:
									** Units: %
									** 3 Rules To Remember:
											1) When an element is fixed, the container of the element is the viewport, so % are relative to that.
											2) A % with an element that has position:absolute; is relative to the ancestor's content+padding. (The ancestor is anything that doesn't have position:static)
											3) A % with an element that is relative or static is relative to it's ancestor's content. (The closest ancestor which is a block level element)
	-Using min-width/h		* To complement a width. We can limit how it resizes when the browser itself resizes by
	 & max-width/height		  placing these limiter properties.

	-Working with "rem"		* Units which are calculated based on the "font" size.
	 and "em"				* Note: Users can change their default font size to make the website font bigger, unless
	 						  we overwrite it with CSS.
							* Syntax: 		font-size: 1.2em;
								** em is multiplied by the default font-size we can change on the browser
							* THE ALARMING THING ABOUT EM is that it multiplies the value we give it with other
							  values it inherits from. WE USE REM when we just want to multiply by the default
							  browser font. (That's why the r in rem stands for ROOT)
							* rem units can also be added to values of margins
							* Default unit is usually 1 rem = 16px

	-Viewport Units "vw"	*These units let us always refer our sizes to the actual viewport
	 & "vh":				*There's also vmin & vmax
	 						*using vw may give you a default scrollbar. You could get rid of it w/ CSS


JAVASCRIPT & CSS:
	-Manipulate CSS with JS by doing:
				1) element.style.someProperty = "someValue";
				2) element.classList.add("someClassName"); //remove()
	-In JS the property names don't have dashes, instead they are in camel case.


MAKING OUR WEBSITE RESPONSIVE:
	-Website Should be made mobile-first (not desktop-first)
	-How can we tell the browser that it should apply a pixel ratio to make sure it translates hardware pixels to the Software pixels.
		* ANSWER: <meta name="viewport" content="width=device-width, inital-scale=1.0">
				  This means that the CSS doesn't consider hardware pixels, but instead software pixels that fit the device

	-Understanding the		*content="width=Device-width, initial-scale=1.0" -  * Tells our browser it's a small device & applies a pixel ratio conversion so the site isn't squeezed in
	"viewport" metatag															* initial-scale applies an initial zoom 
	<meta name=viewport":														* user-scalable=no can be added to prevent zooming in.
																				* maximum/minimum-scale = 2.0 sets the max or minimum zoom

	-Media Queries: 		* Allow us to change the design depending on the actual size (specific properties change depending on the size. Ex: colors, fonts, etc)
							* Design Changes are defined by us
							* Order matters when you use min-width or max-width (ascending or descending values)
							* Media Queries should go at the end of a CSS file

	-Adding our First		* Syntax Example:		@media(min-width: 40rem) {		//(condition): In this case a CSS rule for font-size is executed once the browser has a 40rem width(640px) 
	 Media Query:										#product-overview h1 {
															font-size: 3rem;
														}
													}
							*THINK OF MEDIA QUERY'S AS IFS

	-Logical operators:		* Syntax Examples:
									** "and"						@media (min-width: 40rem) and (min-height: 60rem) {}
																					**SPECIAL THING YOU CAN TARGET IS: orientation: landscape or portrait (not ideal if you want to target desktop)

									** "or"							Same as ^ but seperated by commas instea of and.


Styling Forms:
	-For form inputs you NEED TO specify width: 100%; because otherwise id the display = block; the width would only be taken up by a MARGIN NOT THE BORDER-CONTENT.

	-Advanced Attribute		* Element with Attribute: 							[type] 				
	 Selector:				* Element w/ Specific Attribute Value: 				[type="email"] 		
	 						* Element w/ Specific Atrribute Value in List:		[lang~="en-us"] 	 		//So if there's more whitespace seperated values in this attribute. 
																														It checks if the specified value is in that list
							* Element w/ Specific Attribute Value/Value-:		[lang|="en"]		 		//Checks if the attribute values has the prefix en
							* Element w/ Specific Attribute Value Prefix:		[lang^="#"]			 		//More flexible than the one above because you can have any character
							* Element w/ Specific Attribute Value Suffix:		[href$=".de"]				//For suffixes
							* Element w/ At Least One Attribute Value:			[href$=".de"]				//The ".de" should be somewhere in the value of the attribute href
							* adding i at the end (before ]) would allow case insensitivity

	-Styling the			* New pseduoclass: 							 someSelector:not(anotherSelector)					//This means it excludes anotherSelector from the current rule
	 checkbox:				* Psuedoclasses can go one after another:	.signup-form input:not([type="checkbox"]):focus
	 						* <input> type like checkboxes and lists have a property:	-webkit-appearance:	none:			//WE USE NONE SO THAT WE OVERRIDE IT AND ARE ABLE TO STYLE IT
									** -webkit-appearance is actuallyu specific to chrome so for mozilla we use -moz-appearance: and for others just appearance:
							*When we do override -apearance then we CAN NOT CHECK THE CHECKBOX. Inorder to "reinstall" this capability we USE THE :checked PSEUDO CLASS

	-Validation				*HTML AND JS GIVE US A INVALID PSEUDOCLASS. Syntax:		someSelctor :invalid {}
	 Feedback:						** So it detects things like invalid emails. It can detect this based on the type attribute in tags

WORKING WITH TEXT AND FONTS:
	-Comparing Generic		* Generic Family (parent): serif, sans-serif, cursive, monospace, fantasy
	 Families & Font		* Font Family (child): 		**serif 			- Times New Roman, Georgia
	 Families:											**sans-serif 		- Helvetica, Verdana
	 													**cursive			- Brush Script, Mishal
														**etc..
							* By default the browser fonts Generic Family & Font Family are chosen. Otherwise, We can define a Generic Famiy which allows the broswer to only choose the Font Family.
							  OR FINALLY, we can define a Font Family which can be obtained from:
							  					1) The user's computer
												2) Web fonts
												3) From a server (our own possibly)

	-Understanding			* Syntax:			** "fontFamily1", "fontFamily2", genericFamily; //The font familys apply left to right until one is found otherwise it fallsback to the generic family allowing 
	 font-family:																				  the browser to set the font

	-Google Fonts:			*Go to fonts.google.com
							*You can filter by Generic Families > Then pick a Font Family > And pick a Font Faces > Click "Select This Style" and copy the code into your HTML files where you need it
							*IF YOU WANT THE WEB FONT TO BE AVAILABLE ON ALL PAGES: Use the @import method in that Google Font and paste it to your shared.css (don't copy the style tags)
							*Font faces can not be used in CSS unless your google code defines it. Usually this is comma seperated values of the faces after the link. Same for any other property

	-Font Properties:		* font-variant: small-caps;						** Small characters become capitals
							* font-stretch: ultra-condensed;
							* letter-spacing: 5px;							** Spacing between letters increases
							* white-space: nowrap;							** The entire text will stay as one line and window resizing doesn't affect it (alot more for browser resizing)
							* line-height: 2;								** Increases the height of the content by using the value as a multiple against the font-size (can be px for more absolute)
							* text-decoration: underline;					** Stuff like overline, underline, line-through. And add stuff like dotted, or wavy beside these values and color at the end.
							* text-shadow: 5px 5px 7px rgb(185,180,180);	** The values are offsets to the x and y-axis respectively third arg is a blur and then a color if you want

	-Font shorthand:		* font:  italic		  small-caps	700		   1.2rem/2				"Anonymous Prp", sans-serif
							* font: font-style font-variant font-weight font-size(/line-height) font-family generic-family 
							* or give default system values like: menu or status-bar

	-font-display &			* Values: swap, block, fallback, optional, auto
	 Loading Performance:	* Two different phases block-period and then swap-period 

FLEXBOX:
	-Introduction:			* The modern way to change the way our elements are displayed (another value of the display property)
							* Main aspects:		1) The Flex-container
												2) Main Axis vs. Cross Axis
												3) The Flex Items

	-How we could			* We could get rid of the "hacky" width: calc(100% - 44px)
	 improve our			* It would also help get rid of all of our display: inline-box;'s
	 project using
	 flexbox:

	-Understanding			* When you apply display: flex; to an element it becomes a Flex Container(parent) therefore anything inside it becomes a
	 Flexbox:				  Flex Item (child)	
	 						* Then we can apply properties to the Flex Container and Flex Items:
									** Flex Container Properties+ :
													1) flex-flow:
													2) justify-content:
													3) align-content: 
													4) align-items:
									** Flex Items Properties+:
													1) order:
													2) flex:
													3) align-self:

	-Creating a Flex		* display: flex:
	 Container:					** The child elements align like inline-blocks where they have equal heights (if not defined) according to the element with the biggest height
	 							** The width of the child elements decreases if we decrease the width of the page; ONLY UP TO THE POINT WHERE THE CONTENT OF A CHILD MAY NEED THE SPACE

							* display: inline-flex
								** Similar to flex but the size stay static. If we move the width of the browser the element's don't change. Although, the tallest height thing still happens

	-"flex-direction"		* For display: flex; the automatic values are:		* flex-directiion: row;
	 & "flex-wrap":																* flex-wrap: nowrap;
	 						*We could give them these values:
									*flex-wrap:			**wrap - When we decrease the width of the page, instead of shifting the width of the blocks until they reach their limit. 
																 The blocks start shifting down to the next "row" one at a time. That next row is also the start of a new "leading height"
													
														**wrap-reverse - The same as wrap except it reverse the way things go down to the next "row" and the height is no longer top-down but
																		 instead bottom-up

									*flex-direction:	**column - The elements now behave the way block elements would. Except when you decrease the page width. It shifts up to the content
														**column-reverse - the reverse of column
														**row-reverse - the reverse of row

							* SHORTCUT PROPERTY:		*flex-flow: row wrap;
							

	-Main Axis & 			*Main axis goes from the top-left to the top-right
	 Cross Axis:			*Cross axis goes from the top-left to the bottom-left
	 						*Applying reverse to something makes it go from top-right to top-left (main axis) and top-right to bottom-right (cross axis)
							*So we start minimizing using a normal "wrap", then the elements start shifting from the main axis to the cross axis
							*For columns (the prior 4 *'s were for rows) the same things apply but we reverse the main axis with the cross axis for every case
							* Summary:							Main Axis				Cross Axis
									** row: 					TL -> TR				TL -> BL
									** row-reverse:				TR -> TL				TR -> BR
									** column:					TL -> BL				TL -> TR
									** column-reverse:			BL -> TL 				BL -> BR

	-"align-items" &		* The default property that allows all heights to follow the max height is:		align-items: stretch;
	 "justify-content":				** Other Values:	**center - no more equal heights, but they're aligned to the center
	 													**flex-start - aligns to the cross axis
														**flex-end - alings to the cross axis(but at the end of it) (affects height)

							* "justify-content" has the same values except they relate to the main axis (instead of the cross axis)

	-"align-content":		* align-content: center; - Allows us to align our items along the cross axis
									**space-between  - Leaves a space between the beginning and end of the cross axis when condensing the width

	-Flexbox Items			* order:					** Explanation: 	Changes the order of an element
	 Properties:										** Values: 			1 (someNumber that describes the order. All start at 0 by default. Anything higher puts it at the end of the list, lower puts it at the front)

	 						* align-self:				** Explanation: 	Positions the element in relation to the Cross Axis as align-items would. Except the alignment only applies to this single Flex Item
														** Values:			Everything 'align-items' has

							* flex-grow: 				** Explanation: INT's where all of the children of the parent flexbox that have this. Declare it's own fraction of what it has. So 4 grows 4 times as faster
														** values:	INT's

							* flex-shrink:				** Explanation: Default value is 1. 0 doesn't shrink elements. If one element has value 1, and another 4. Then the fraction of both decides the speed of the shrink.
														** Values: INT's

							* flex-basis:				** Explanation: Defines the size of an element depending on the main axis (it overrides the width property if set to row. And the height if set to column)
																		It fallsback to the standard width/height property if thr value of flex-basis is auto (because it follows the main axis)
																		% is dependent of the size of the flex container
														** Values: px, %
														** Shorthand:		flex: grow shrink basis;  //These are property names not values. Replace with values.


CSS GRID:
	-What is the			* A grid-based layout tool that allows you to define rows and columns and where elements can be placed within it
	 CSS Grid?:	

	-Turning an				* display: grid;
	 element into						** This turns the element into a grid and it's DIRECT CHILDREN are positioned within the grid. (and are considered in the Dev Tools Row Visual Mechanism)
	 a Grid:							** The Browser Dev Tools have visualization for CSS grid in Layout>Grid

	-Defining columns		* Where the grid was defined you can add 'grid-template-columns' to override the default 1 column that the grid begins with.
	 & rows:				* The values of 'grid-template-colums' are back-to-back sizes for as many columns as you want. 
	 								** EX: 		grid-template-columns: 200px 150pc 20%; //Yields thre columns. The first two w/ absolute sizes and the last is relative to the grid container
							* We can use the special unit 1fr. Where fr takes up the remaining space, but if there's another fr like 2fr, then all fr's take up the remaining space but 2fr gets twice
							  as much of the remaining space as 1fr
							* Another value is auto. For columns it takes the remaining available width, and for rows it takes as much space as it needs to fit the content
							* TO REPEAT THE SAME SIZE FOR MANY COLS/ROWS USE the repeat() function as a value:		repeat(4, 25%); //yields 4 columns w/ size 25%
																																	//You can put a name infront of 25% w/ [col-start] and behind [col-end]
																																	  then later on reference them with grid-column: col-start 2 / col-end 2;
																													minmax(10px, 200px); //Defines a range of the size we could put if space is available

							*WE CAN ALSO EXPLICITLY DEFINE ROWS (in the same location as columns): using 'grid-template-rows'. The values work the same as columns (define sizes for as many rows as you want)

	-Positioning Child		* By default each element takes 1 cell in the grid
	 Elements in a Grid:	* We can define how many cells an element takes (as well as where it's positioned) by inserting the following properties into a child element of the grid:
										1) grid-column-start: 3		//CSS grid has numbered lines for row/cols. You can use your browser Dev Tools to visualize them
										2) grid-column-end: 5	//By default it's always +1 of start
										** This pushes any element that was placed there to the next row/col
							* We can do the same for rows using: 
										1) grid-row-start: 1
										2) grid-row-end: 3

	-Advanced Element		* Instead of using a "hard-coded by line" 'grid-column-end" we can use:		span 2;		//Offsets by 2 from the start, hence spanning 2 cells from the start
	 Positioning: 			* Negative values are units starting from the end of the grid
	 						* It is possible for elements to overlap if you explicitly set all positions (otherwise it may dynamically look for a different position to prevent overlaps)
									** In cases of overlap the DOM order matters. The one that comes ahead in the DOM is on top. THOUGH U CAN CHANGE THIS WITH Z-INDEX

	-Working with named		* Instead of using numbered lines we can use named lines.
	 lines: 				* You provide names for lines when creating the rows/cols as follows:
	 								** grid-template-rows: [someName] 5rem [someName1] 5rem [someName2] 100px; //...etc
												*** In the brackets you can add multiple names by seperating the with a SPACEBAR 
												*** Convention says we name as follows: [row-1-start] sizeVal [row-1-end row-2-start] sizeVal [row-2-end row-3-start] sizeVal [col-3-end]
												*** After naming, you can reference the names in places where you define your start and end positions

	-Column & Row			* grid-column-start & end can just be - 'grid-column' w/ the following shorthand:		grid-column: start / end	//where we replace start and end with actual numbers
	 Shorthands			 		** Works the same with 'grid-row'
	 						* There's a also a shorthand that encapsulates all 'Grid Item' properties:
													** grid-area: row-start / col-start / row-end / col-end;  //Where your replace the words with actual indeces on the Grid

	-Working w/ Gaps:		* grid-column-gap: 20px;
									** This defines gaps between each column
							* grid-row-gap: 20px; //works similarly ^
							* A shorthand: grid-gap: row col; //replace for actual size values
												** If you only specify one value here. It means same gaps for all

	-Adding Named			* In our Grid Container we can define the property(after defining rows & cols of our grid):		grid-template-areas		//This property will let us name cells in this container
	 Template Areas					**VALUE: A string with a pattern (You must know how you defined your layout). The following example shows a Grid w/ 4C & 3R
									**EX:	grid-template-areas:	"header header header header" //4 Cols
																	"side side main main"		  //3 Rows
																	"footer footer footer footer";
									*** To define empty cells in the EX above simply put down a "." for a cell
							* Now for some Grid Item we can use:
													grid-area: header;
							  AND IT WILL KNOW WHERE TO GO BASED ON the 'grid-template-areas- WE DEFINED IN THE GRID CONTAINER
							*Grid area does not respect the DOM

	-Using the Grid			* Any Grid container child element that is not part of the "flow" IS NOT put into the grid
	 on our Project:

	-fit-content(8rem)		* A useful value when it comes to having some row that should have some minimum size but also not be bigger than its content
							* We put it as the size of the last row (the footer) so the Grid row can fit it perfectly

	-Positioning Grid		* By default the elements take up the whole area of a Grid cell. But there are properties that can change this. They must be placed where the Grid container was defined:
	 Elements:							1) justify-items:		* center - the elements become centered to their "area"
	 															* start - moves the element to the start of their area
																* end - moves the element to the end of their area
																* stretch - the default value

										2) align-columns:		* Exactly the same as justify-items except its for COLUMNS in the "area"

	-Positioning the		* justify-content:	*Same values as above but for all of the content in the Grid  //X-AXIS (Row)
	 entire grid			* align-content:	*Y-AXis (col)
	 content:

	-Positioning			* First go to the rule of the individual element you want to reposition and use:
	 Elements 						** justify-self: center, start, end, etc... (X-AXIS)
	 Individually:					** align-self: same ^ (Y-AXIS)

	-Understanding 			* Literally just update the row and column layouts / areas
	 Responsive Grids:
	 (Grid+Media Queries)

	-Applying Autoflow:		* The Grid automatically adds rows after our defined rows if needed.
								** The size of these dynamic rows are "as big as they neeed to be" by default. (the biggest controls the height of the row)
								** WE COULD OVERRIDE THIS WITH-		grid-auto-rows: 30rem;	(the default value WAS auto)
							* To generate dynamic columns instead of rows use:
										grid-auto-flow: column (WAS row BY DEFAULT)
								**Then we'd also be able to size the column generation with: grid-auto-columns: 30rem;

	-"auto-fill" &			* grid-template-columns: repeat(auto-fill, 10rem);
	 "auto-fit"						** Ensures that it fills the current row with as many items as possible and then it will "wrap" and enter a new row.
	 						* If we replace auto-fill above with auto-fit, it will have the same behaviour except that auto-fit also centers the items

	-Creating a Dense		* grid-auto-flow: row dense; 			//this is the property that can have the value column dense
	 Grid:							** dense overwrites elements not positioning themselves to fill up gaps because of
	 									their size and their respect of the DOM. So now there shouldn't be gaps!
									** Though this may not be optimal. Because screen readers still respect the DOM

TRANSFORMING ELEMENTS WITH CSS TRANSFORMS:
	-Transforms are hardware accelerated

	-Rotating Elements		* Syntax(Rotate):				transform: rotateZ(45deg);
	 and setting			* To define
	 'transform-origin':      rotation origin:				transform-origin: left top;		//by default it's just center. Can also be values of px or % or rem


	-Using Rotate and		* Syntax(+Translate):			transform: rotateZ(45deg) translateX(1rem) tranlsateY(1rem); //So it moves our previous translation to the right/down by 1rem
	 Translate:			 	* TRANSLATIONS ARE LOCAL TO THE CENTER OF THE "BOX" OF THE ELEMENT
	 						* overflow: hidden on the parent of the translated element would be good to hid things that are transformed beyond it.

	-Working w/ "skew"		* "skew" and "scale" are function values of 'transform'
	 and "scale"					** skew(xVal, yVal)
	 										** If you also do a skew on the child. You can revert the child to it's original skew all the while keeping the skew of the parent
									** scale(2)
											** the 2 here would mean double the size of the original size on both the x and y axis

	-Rotating Elements		* Rotation along the X and Y axis is 3D (it's hard to see because we are in "far perspective" by default)
	 in 3D(and using		* using perspective(500px) we say "the smaller the perspective is, the closer you are to the element". This only affects the element where the function is used.
	 'perspective'):				** PERSPECTIVE CAN BE A PROPERTY WHICH SHOULD BE APPLIED TO THE PARENT. +it gets applied to all children 
	 						* Change the angle of the perspective by using: 		perspective-origin: right (can be %, rem, px)
							* 'transform-style' has the default flat value which is bad if we rotate the parent
											** We use the 'preserve-3d' value to not make children invisible depending on a transformation 
							* 'backface-visibilty' erases elements that are showing their "backface"
	 
TRANSITIONS AND ANIMATIONS IN CSS:
	-'transition' is a property that lets you "watch other properties" and set timing functions/animations.

	-'transition'			* transition: opacity 0.2s ease-in 1s, transform 0.5s
	  Syntax:							 ** Each property we watch (as in watch for JS to change the property) has an animation time for the transition to finish and you can specify ease-in (or out)
	  										to start the animation slow and end fast. The time after ease-in means nothing happens for 1 second and then it plays the animation

	-CSS Timing 			* Google CSS Timing Functions for a helpful cheat sheet
	 Functions:

	 -Transitions			* In elements where you both use transition and display, display overrides the transition effect
	  and display:					** We can't use an opacity transition because the opacity is just invisible but still clickable, but we may need it to not be clickable hence the need for display: none.
	  						* In order to solve this problem, we can set an inline display style and immidiately after, a JS timeOut().Hence, we let the display change first and after 10ms we us our transition
							* OF COURSE A CLEVER WAY TO PREVENT THIS IS TRANSLATING IT AWAY FROM THE VIEWPORT USING AN INITIAL TRANSLATION (which is flowless) of -100% (or positive) on some axis
							* THE TRANSFORM PROPERTY OF ANIMATIONS OVERRIDE ELEMENT TRANSFORMATIONS AS LONG AS THE ANIMATION IS PLAYED.

	-USING CSS				* You have way more control over the animation as opposed to transitions
	 ANIMATIONS (think		* Syntax of a Keyframe:
	 of them as CSS									@keyframes someKeyFrameName {
	 TRANSITIONS++)										from {
															transform: rotateZ(0); //initial state
														}
														to {
															transform: rotateZ(10deg);
														}
													}
									** Note: there are no selectors in from or to. It later gets applied to elements that receive this keyframe as an animation
								
							* Syntax for the animation property on a rule we want to apply the animation to:
													animation: someKeyFrameName, 200ms, 3s, 8 alternate;		//Where args after someKeyFrameName are - animationTime, delayUntilPlay, numberToRepeat (respectively)
																												//If alternate is defined then it takes 1 of the 8 iterations here to move back to the starting state
																												  otherwise it would just "snap back" to the starting state

							* You can tell the animation which values to keep with the FILL STATE.
													** simply add the keyword forwards to keep the value of the to{} frame. Backwards to keep the value of the original state of the element (not from{})
													   THIS IS USEFUL FOR HOVERING STATES. WHICH KEEPS THE FINAL VALUE OF THE ANIMATION AS LONG AS YOUR MOUSE HOVERS OVER SOMETHING

							* Summary:			animation: NAME DURATION DELAY TIMING-FUNCTION ITERATION DIRECTION FILL-MODE PLAY-STATE;
											(ex)animation: wiggle 200ms 1s ease-out 8 alternate forwards running; 

											 "Play the wiggle keyframe set (animation) over a duration of 200ms. Between two keyframes start fast and end slow, also make sure to wait 1s before you start. 
											 Play 8 animations and alternate after each animation. Once you're done, keep the final value applied to the element. Oh, and you should be playing the animation - not pausing."


	-Adding Multiple		*To add multiple keyframes. Instead of using from and to, we can instead use as many percentages from 0% to 100% to define keyframes
	 Keyframes:

	 -JS Animation			*We can add eventListeners that pertain to animations of an element by using the following event on the first argument:
	  Event Listener:					1) 'animationstart'
	  									2) 'animationend'
										3) 'animationiteration'

WRITING FUTURE-PROOF CSS CODE:
	-Usign CSS 				* For situations where we have a value that is repeated a lot.
	 Variables:				* For a color value we would define:
	 										:root {						(:root refers to the entire doc)
												--some-var-name: #ffff;
											}
							  and then be able to use it as a value anywhere else with:
							  				var(--some-var-name);
							  a second argument in var() can be used as a fallbcack value

	-Which prefixes			* Every browser has its own prefix. To support independent module implementation.
	 should you use?:		* You can use the prefix /w a property, by repeatedly defining the property
	 						  and defining the standard property at the end which should use that final 
							  property if the browser has support for it, otherwise it uses its prefix.
							* GITHUB ATUOPREFIXER is a tool to do this automatically. There are also online 
							 versions where you can paste your code and it generates the prefixed version.

	-Detecting browser		* Syntax:		@supports (property: value)
	 support w/ @supports:					{
												//Insert Rules to execute here.
	 										}
							 This checks if the propert: value pair is supported and if it is, it executes
							 the rules within the {}
							 * You can use and, or, not keywords after the parenthesis to append more conditions.

	-Pollyfills:			* Definition: A polyfill is a JS Package which enables certain CSS Features
										  in Browsers which would not support it otherwise.
							* Polyfills are computationally expensive because they have to be loaded & parsed.

	-Eliminate Cross		* Different browsers have different defaults (font sizes, paddings, box sizes, etc...)
	 Browser 				* Therefore, you can implement some "reset libraries" like Normalize.css which 
	 Inconsistencies:		  should be the first import in your html files (like our * {} where we set 
	 						  box-sizing: border-box;)

	-CSS Classes			* Use kebab case (dashes) since CSS is case insensitive
	 naming conventions:	* Name by feature not style. The styling could be read from the properties we use.
	 						* Block Element Modifier Style (BEM):
										.BLOCK__ELEMENT--MODIFIER
								** Ex: .menu-main__item--size-big

	-Vanilla CSS vs			* Component Frameworks: Bootstrap
	 Frameworks:			* Fast components but they may look the same to other websites

INTRODUCING SASS (Synctactically Awesome Style Sheets)
	-What is SASS &			* SASS does not run on the browser
	 SCSS?								INSTEAD
	 						  It extends CSS during development only.
							  THEREFORE, it has to be compiled to normal CSS before production.
FINISHED! JULY 4!!