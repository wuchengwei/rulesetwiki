
Converter "X-lint" RuleSet Wiki
================================


### Introduction
  In order to help developer build apps with good responsive design, we introduce some rules that indicates what's the "Anti Pattern" and what X-lint thinks might be the "Best Practice". In the following document, we will explain in detail about these rules. We may give sample code for some rules.
### Category
#### [Mulitimedia](#multi)
##### [Low image quality in high resolution screen](#multi1)
##### [High image quality in low resolution screen](#multi2)
##### [Detect non-compatible format of images, audio and video for target](#multi3)
##### [Use non-compatible ways to play audio or videos (Flash etc.)](#multi4)
#### [CSS](#css)
##### [Use vendor prefixed CSS styles](#css1)
##### [Use absolute units in CSS](#css2)
##### [Use 2D transform for 3D hardware accelerated platform](#css3)
##### [Find font-size/font](#css4)
##### [Use missing styles for target platform](#css5)
#### [JS](#js)
##### [Use consecutive multiple styles instead of class in scripts](#js1)
##### [Use platform dependent Web Runtime APIs](#js2)
#### [HTML](#html)
##### [Use consecutive multiple styles instead of class](#html1)
##### [Viewport misuse](#html2)



<div id="multi"></div>
<div id="multi1"></div>
#### Multimedia
##### Low image quality in high resolution screen
The following picture shows a typical example：

![alt text](images/iphoneblur.jpg "low image quality")

Although the screen size of Iphone4S is 960*640 with retina display, the photo shown is of low quality and hard to recognize.

When meeting this kind of situation, the X-lint will promote to provide high resolution images.

<div id="multi2"></div>
##### High image quality in low resolution screen
This situation is the opposite of the former. Although in this situation the image shown won't be hard to recognize, it indeed cost unnecessary bandwidth.

When meeting this kind of situation, the X-lint will promote to compress images for low resolution

<div id="multi3"></div>
#### Detect non-compatible format of images, audio and video for target
The images format of JPEG,GIF and PNG can be almost displayed in all browsers, but there are some other image formats that might not be supported in some browser:
	
<pre><code><font color="red">&lt;!-- Opera doesn't support '.tiff' --&gt;
&lt;!-- X-lint will promote users to provide other image formats --&gt;
&lt;img src="boat.tiff" alt="Big Boat" /&gt;</font>
</code></pre>
	
* For more information about [image format support](http://en.wikipedia.org/wiki/Comparison_of_web_browsers#Image_format_support)。

The tag '\<audio>' is a newly defined element in HTML5 ,it specifies a standard way to embed an audio file on a web page. Currently, there are 3 supported file formats for the '\<audio>' element: MP3, Wav, and Ogg. But the supportability of the 3 file formats is not the same in different browsers. X-lint will check the compatibility of audio format for target, and suggested that user provided different formats for compatibility:
	
<pre style="background-color:#fff8f8"><code ><font color="red" style="background-color:#fff8f8">&lt;!-- Anti Pattern --&gt;
&lt;!-- suppose the target is IE9 which doesn't support '.ogg' --&gt;
&lt;audio controls="controls"&gt;
  &lt;source src="song.ogg" type="audio/ogg" /&gt;
&lt;/audio&gt;
&lt;!-- X-lint will suggested that user provided different formats for compatibility --&gt;  
</font></code></pre>
	
X-lint will provide a good pattern like:
	
<pre style="background-color:#f8fff8"><code><font color="green" "style="background-color:#f8fff8"">&lt;!-- Good Pattern --&gt;
&lt;!-- The browser will use the first recognized format --&gt;
&lt;audio controls="controls"&gt;
  &lt;source src="song.ogg" type="audio/ogg" /&gt;
  &lt;source src="song.wav" type="audio/wav" /&gt;
  &lt;source src="song.mp3" type="audio/mp3" /&gt;
&lt;/audio&gt; 
</font></code></pre>

The tag '\<video>' is pretty much the same as the tag '\audio' :
	
<pre><code><font color="red">&lt;!-- Anti Pattern --&gt;
&lt;!-- suppose the target is IE which doesn't support '.ogg' --&gt;
&lt;video width="320" height="240" controls="controls"&gt;
  &lt;source src="movie.ogg" type="video/ogg" /&gt;
&lt;/video&gt;
&lt;!-- X-lint will suggested that user provided different formats for compatibility --&gt;  
</font></code></pre>

X-lint will provide a good pattern like:
	 
<pre><code><font color="green">&lt;!-- Good Pattern --&gt;
&lt;!-- The browser will use the first recognized format --&gt;
&lt;video width="320" height="240" controls="controls"&gt;
  &lt;source src="movie.mp4" type="video/mp4" /&gt;
  &lt;source src="movie.ogg" type="video/ogg" /&gt;
 &lt;/video&gt;
</font></code></pre>

* For more information about [video format support](http://en.wikipedia.org/wiki/HTML5_video#Browser_support).


<div id="multi4"></div>
#### Use non-compatible ways to play audio or videos (Flash etc.)
There are some platforms that doesn't support some ways to play audio or videos,or it is impossible for the platform to install FLash plugin. For example, Iphone and Ipad don't support Flash:
	
<pre><code><font color="red">&lt;!-- Iphone and Ipad don't support Flash --&gt;
&lt;embed 
  src="hello.swf" quality="high" bgcolor="#000000" width="320" height="200"&gt; 
&lt;/embed&gt;
</font></code></pre>
	
X-lint will provide HTML5 ways of playing audio and video :
	 
<pre><code><font color="green">&lt;!-- HTML5 ways of playing audio and video --&gt;
&lt;audio controls="controls"&gt;
  &lt;source src="hello.ogg" type="audio/ogg" /&gt;
  &lt;source src="hello.wav" type="audio/wav" /&gt;
  &lt;source src="hello.mp3" type="audio/mp3" /&gt;
&lt;/audio&gt; 
&lt;video width="320" height="240" controls="controls"&gt;
  &lt;source src="hello.mp4" type="video/mp4" /&gt;
  &lt;source src="hello.ogg" type="video/ogg" /&gt;
 &lt;/video&gt;
</font></code></pre>

<div id="css"></div>
<div id="css1"></div>
#### CSS
##### Use vendor prefixed CSS styles
The user may hate writing vendor prefixes for different browsers which is necessary for different browsers to run the code correctly.When X-lint detect vendor prefixed CSS styles, it will provide standard as well as other vendor prefixed styles for it:
	
<pre><code><font color="red">&lt;!-- here are some vendor prefixed CSS styles without --&gt;
&lt;!--providing standard and othervendor prefixed styles --&gt;

div {-moz-border-radius: 10px;}
div {-webkit-border-radius: 20px;}
#sample1 {-moz-box-shadow: -5px -5px 5px;}
div {-webkit-border-bottom-left-radius:2em;}
div {-ms-layout-flow: horizontal;}
</font></code></pre>
	
X-lint will proveide :
    
<pre><code><font color="green">div {
-moz-border-radius: 10px;
-webkit-border-radius: 10px;
border-radius: 10px
}

div {
-moz-border-radius: 20px;
-webkit-border-radius: 20px;
border-radius: 20px
}

#sample1 {
-moz-box-shadow: -5px -5px 5px;
-webkit-box-shadow: -5px -5px 5px;
box-shadow: -5px -5px 5px
}

div {
-webkit-border-bottom-left-radius: 2em;
-moz-border-radius-bottomleft: 2em;
border-bottom-left-radius: 2em
}

div {
-ms-layout-flow: horizontal
}
</font></code></pre>

* For more information about [vendor prefixed css](http://peter.sh/experiments/vendor-prefixed-css-property-overview/).

<div id="css2"></div>
##### Use absolute units in CSS
X-lint tend to use relative rather than absolute units. This way the content of a page will adjust better to the browser window and fonts will be displayed relative to the users specifications or relative to the default settings of the browser. But this may not always be the case, in some situation, it might be better to use absuloute units:
	
<pre><code><font color="red">&lt;!-- these are some absolute units that X-lint will warn users --&gt;
&lt;!-- it is hard to adjust for multiple screens --&gt;
h1 { margin: 0.5in;}      /* inches  */
h2 { line-height: 3cm;}   /* centimeters */
h3 { word-spacing: 4mm;}  /* millimeters */
h4 { font-size: 12pt;}    /* points */
h4 { font-size: 1pc;}     /* picas */
p  { font-size: 12px;}    /* px */
</font></code></pre>
	
X-lint tend to use :
	
<pre><code><font color="green">&lt;!-- these are some relative units that X-lint tend to use them --&gt;
h1 {line-height: 1.2em }
h2 {margin: 2ex}
h3 {wird-spacing: 3ch}  
</font></code></pre>
	
<div id="css3"></div>
##### Use 2D transform for 3D hardware accelerated platform
Some platforms such as Firefox and Chrome support 3D transform, X-lint will provide 3D transform instead of 2D if detected：
	
<pre><code><font color="red">&lt;!-- Suppose the platform is Chrome which supports 3D transform --&gt;
&lt;!-- these are 2D transform, X-lint will provide 3D transform  --&gt;
div
{
  transform: translate(50px,100px);
  -ms-transform: translate(50px,100px); /* IE 9 */
  -webkit-transform: translate(50px,100px); /* Safari and Chrome */
  -o-transform: translate(50px,100px); /* Opera */
  -moz-transform: translate(50px,100px); /* Firefox */
}
</font></code></pre>

X-lint will provide:

<pre><code><font color="green">&lt;!-- X-lint provides 3D tranform --&gt;
div
{
  transform: rotateX(50px);
  -webkit-transform: rotateX(50px); /* Safari and Chrome */
  -moz-transform: rotateX(50px); /* Firefox */
  transform: rotateY(50px);
  -webkit-transform: rotateY(50px); /* Safari and Chrome */
  -moz-transform: rotateY(50px); /* Firefox */
}
</font></code></pre>

<div id="css4"></div>
##### Find font-size/font
fond-size/font may change the layout of a site considerably. Different browsers interpret font sizes differently, so a font that appears readable in IE may be smaller when viewed in Chrome. In addition, font sizes on different platforms are not always the same. X-lint tend to specify a font size in pixels (px) not points (pt) or em. Using a pt or em font-size property instead of px allows for the site text to be resized according to the viewer's system settings. If their system is set to view very large text, your web site's layout will become distorted and your web site may be illegible to them. 

Also, user may  set the font-size pixels too small. Some people may not be able to read tiny text and adjusting their system text size will have no effect on the site because the font-size is specified as px. And X-lint will provide a recommended font-size for target device.

<div id="css5"></div>
##### Use missing styles for target platform
Some platform may not support some styles. When detect this problem, X-lint will provide a fallback or warn users to change it manually:

<pre><code><font color="red">&lt;!-- IE --&gt;
&lt;!-- IE doesn't support 'outline' --&gt;
&lt;!-- X-lint will provide a fallback or warn users to change it manually
p {
  border:1px solid red;
  outline:green dotted thick;
｝
</font></code></pre>
	
	
<div id="js"></div>
<div id="js1"></div>
#### JS
##### Use consecutive multiple styles instead of class in scripts
X-lint considers manipulating CSS styles in JavaScript code an anti pattern :
	
<pre><code><font color="red">&lt;!-- JavaScript Code --&gt;
&lt;!-- the code tries to manipulate CSS styles directly--&gt; 
function changeCSS(){
  document.getElementById("t").style.color= "red";
  document.getElementById("t").style.fontSize= "30px";
  document.getElementById("t").style.backgroundColor = "blue";
}
</font></code></pre>

X-lint will provide extracted CSS class of the change so that user can manipulate the CSS styles by changing the tag's class, which X-lint considers a best practice: 

	
<pre><code><font color = "green">&lt;!-- X-lint will provide extracted class in CSS for user to manipulate --&gt;
.converted {
  color: red;
  font-size: 30px;
  background-color: blue;
}
</font></code></pre>
	
User then can manipulate the style using JavaScript like:
	
<pre><code><font color="green">&lt;!-- Use JavaScript to change the class of the tag instead of manupulating CSS style directly--&gt;
function changeCSS() {
  document.getElementById("t").className = "converted";
}
</font></code></pre>
	
<div id="js2"></div>
##### Use platform dependent Web Runtime APIs


<div id="html"></div>
<div id="html1"></div>
#### HTML
##### Use consecutive multiple styles instead of class
X-lint considers in-line CSS an anti pattern. When detect this problem, X-lint will provide extracted classes:

	
<pre><code><font color="red">&lt;!-- X-lint considers in-line CSS an anti pattern --&gt;
&lt;p style="font-size: 10px; color: #FFFFFF;background: blue; "&gt;
  Hello! 
&lt;/p&gt;
</font></code></pre>
	
<br/>

	
<pre><code><font color="green">&lt;!-- extracted class in CSS --&gt;
p {
  font-size: 10px;
  color: #FFFFFF;
  background: blue;
}
</code></pre>
	
<div id="html2"></div>
##### Viewport misuse

    