---
title: Canvas API
slug: Web/API/Canvas_API
tags:
  - API
  - Canvas
  - Graphics
  - JavaScript
  - Overview
  - Reference
---
<div>{{CanvasSidebar}}</div>

<p>The <strong>Canvas API</strong> provides a means for drawing graphics via <a href="/en-US/docs/Web/JavaScript">JavaScript</a> and the <a href="/en-US/docs/Web/HTML">HTML</a> {{HtmlElement("canvas")}} element. Among other things, it can be used for animation, game graphics, data visualization, photo manipulation, and real-time video processing.</p>

<p>The Canvas API largely focuses on 2D graphics. The <a href="/en-US/docs/Web/API/WebGL_API">WebGL API</a>, which also uses the <code>&lt;canvas&gt;</code> element, draws hardware-accelerated 2D and 3D graphics.</p>

<h2 id="Basic_example">Basic example</h2>

<p>This simple example draws a green rectangle onto a canvas.</p>

<h3 id="HTML">HTML</h3>

<pre class="brush: html">&lt;canvas id="canvas"&gt;&lt;/canvas&gt;
</pre>

<h3 id="JavaScript">JavaScript</h3>

<p>The {{domxref("Document.getElementById()")}} method gets a reference to the HTML <code>&lt;canvas&gt;</code> element. Next, the {{domxref("HTMLCanvasElement.getContext()")}} method gets that element's context—the thing onto which the drawing will be rendered.</p>

<p>The actual drawing is done using the {{domxref("CanvasRenderingContext2D")}} interface. The {{domxref("CanvasRenderingContext2D.fillStyle", "fillStyle")}} property makes the rectangle green. The {{domxref("CanvasRenderingContext2D.fillRect()", "fillRect()")}} method places its top-left corner at (10, 10), and gives it a size of 150 units wide by 100 tall.</p>

<pre class="brush: js">const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

ctx.fillStyle = 'green';
ctx.fillRect(10, 10, 150, 100);
</pre>

<h3 id="Result">Result</h3>

<p>{{ EmbedLiveSample('Basic_example', 700, 180) }}</p>

<h2 id="Reference">Reference</h2>

<ul>
 <li>{{domxref("HTMLCanvasElement")}}</li>
 <li>{{domxref("CanvasRenderingContext2D")}}</li>
 <li>{{domxref("CanvasGradient")}}</li>
 <li>{{domxref("CanvasImageSource")}}</li>
 <li>{{domxref("CanvasPattern")}}</li>
 <li>{{domxref("ImageBitmap")}}</li>
 <li>{{domxref("ImageData")}}</li>
 <li>{{domxref("TextMetrics")}}</li>
 <li>{{domxref("OffscreenCanvas")}} {{experimental_inline}}</li>
 <li>{{domxref("Path2D")}} {{experimental_inline}}</li>
 <li>{{domxref("ImageBitmapRenderingContext")}} {{experimental_inline}}</li>
</ul>

<div class="notecard note">
<p><strong>Note:</strong> The interfaces related to the <code>WebGLRenderingContext</code> are referenced under <a href="/en-US/docs/Web/API/WebGL_API">WebGL</a>.</p>
</div>

<div class="notecard note">
<p><strong>Note:</strong> {{domxref("OffscreenCanvas")}} is also available in web workers.</p>
</div>

<p>{{domxref("CanvasCaptureMediaStreamTrack")}} is a related interface.</p>

<h2 id="Guides_and_tutorials">Guides and tutorials</h2>

<dl>
 <dt><a href="/en-US/docs/Web/API/Canvas_API/Tutorial">Canvas tutorial</a></dt>
 <dd>A comprehensive tutorial covering both the basic usage of the Canvas API and its advanced features.</dd>
 <dt><a href="https://joshondesign.com/p/books/canvasdeepdive/title.html">HTML5 Canvas Deep Dive</a></dt>
 <dd>A hands-on, book-length introduction to the Canvas API and WebGL.</dd>
 <dt><a href="https://bucephalus.org/text/CanvasHandbook/CanvasHandbook.html">Canvas Handbook</a></dt>
 <dd>A handy reference for the Canvas API.</dd>
 <dt><a href="/en-US/docs/Web/API/Canvas_API/A_basic_ray-caster">Demo: A basic ray-caster</a></dt>
 <dd>A demo of ray-tracing animation using canvas.</dd>
 <dt><a href="/en-US/docs/Web/API/Canvas_API/Manipulating_video_using_canvas">Manipulating video using canvas</a></dt>
 <dd>Combining {{HTMLElement("video")}} and {{HTMLElement("canvas")}} to manipulate video data in real time.</dd>
</dl>

<h2 id="Libraries">Libraries</h2>

<p>The Canvas API is extremely powerful, but not always simple to use. The libraries listed below can make the creation of canvas-based projects faster and easier.</p>

<ul>
 <li><a href="https://www.createjs.com/easeljs">EaselJS</a> is an open-source canvas library that makes creating games, generative art, and other highly graphical experiences easy.</li>
 <li><a href="http://fabricjs.com">Fabric.js</a> is an open-source canvas library with SVG parsing capabilities.</li>
 <li><a href="https://www.patrick-wied.at/static/heatmapjs/">heatmap.js</a> is an open-source library for creating canvas-based data heat maps.</li>
 <li><a href="https://thejit.org/">JavaScript InfoVis Toolkit</a> creates interactive data visualizations.</li>
 <li><a href="https://konvajs.github.io/">Konva.js</a> is a 2D canvas library for desktop and mobile applications.</li>
 <li><a href="https://p5js.org/">p5.js</a> has a full set of canvas drawing functionality for artists, designers, educators, and beginners.</li>
 <li><a href="http://paperjs.org/">Paper.js</a> is an open-source vector graphics scripting framework that runs on top of the HTML5 Canvas.</li>
 <li><a href="https://phaser.io/">Phaser</a> is a fast, free and fun open source framework for Canvas and WebGL powered browser games.</li>
 <li><a href="https://ptsjs.org">Pts.js</a> is a library for creative coding and visualization in canvas and SVG.</li>
 <li><a class="link-https" href="https://github.com/jeremyckahn/rekapi">Rekapi</a> is an animation key-framing API for Canvas.</li>
 <li><a href="https://scrawl.rikweb.org.uk/">Scrawl-canvas</a> is an open-source JavaScript library for creating and manipulating 2D canvas elements.</li>
 <li>The <a href="https://zimjs.com">ZIM</a> framework provides conveniences, components, and controls for coding creativity on the canvas — includes accessibility and hundreds of colorful tutorials.</li>
</ul>

<div class="notecard note">
<p><strong>Note:</strong> See the <a href="/en-US/docs/Web/API/WebGL_API">WebGL API</a> for 2D and 3D libraries that use WebGL.</p>
</div>

<h2 id="Specifications">Specifications</h2>

{{Specifications("html.elements.canvas")}}

<h2 id="Browser_compatibility">Browser compatibility</h2>

<p>Mozilla applications gained support for <code>&lt;canvas&gt;</code> starting with Gecko 1.8 (<a href="/en-US/docs/Mozilla/Firefox/Releases/1.5">Firefox 1.5</a>). The element was originally introduced by Apple for the OS X Dashboard and Safari. Internet Explorer supports <code>&lt;canvas&gt;</code> from version 9 onwards; for earlier versions of IE, a page can effectively add support for <code>&lt;canvas&gt;</code> by including a script from Google's <a href="https://github.com/arv/explorercanvas">Explorer Canvas</a> project. Google Chrome and Opera 9 also support <code>&lt;canvas&gt;</code>.</p>

<h2 id="See_also">See also</h2>

<ul>
 <li><a href="/en-US/docs/Web/API/WebGL_API">WebGL</a></li>
</ul>