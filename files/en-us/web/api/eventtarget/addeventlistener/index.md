---
title: EventTarget.addEventListener()
slug: Web/API/EventTarget/addEventListener
tags:
  - API
  - AccessOuterData
  - DOM
  - Detecting Events
  - Event Handlers
  - Event Listener
  - EventTarget
  - JavaScript
  - Method
  - PassingData
  - Receiving Events
  - Reference
  - addEventListener
  - attachEvent
  - events
  - mselementresize
browser-compat: api.EventTarget.addEventListener
---
<p>{{APIRef("DOM Events")}}</p>

<p>The {{domxref("EventTarget")}} method
    <strong><code>addEventListener()</code></strong> sets up a function that will be
    called whenever the specified event is delivered to the target.</p>

<p>Common targets
  are {{domxref("Element")}}, {{domxref("Document")}}, and {{domxref("Window")}}, but the
  target may be any object that supports events (such as {{domxref("XMLHttpRequest")}}).
</p>

<p><code>addEventListener()</code> works by adding a function or an object that implements
  {{domxref("EventListener")}} to the list of event listeners for the specified event type
  on the {{domxref("EventTarget")}} on which it's called.</p>

<h2 id="Syntax">Syntax</h2>

<pre class="brush: js">target.addEventListener(type, listener);
target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture);
target.addEventListener(type, listener, useCapture, wantsUntrusted); // wantsUntrusted is Firefox only
</pre>

<h3 id="Parameters">Parameters</h3>

<dl>
  <dt><code><var>type</var></code></dt>
  <dd>A case-sensitive string representing the <a href="/en-US/docs/Web/Events">event
      type</a> to listen for.</dd>
  <dt><code><var>listener</var></code></dt>
  <dd>The object that receives a notification (an object that implements the
    {{domxref("Event")}} interface) when an event of the specified type occurs. This must
    be an object implementing the {{domxref("EventListener")}} interface, or a JavaScript
    <a href="/en-US/docs/Web/JavaScript/Guide/Functions">function</a>. See
      {{anch("The event listener callback")}} for details on the callback itself.</dd>
  <dt><code><var>options</var></code> {{optional_inline}}</dt>
  <dd>An options object specifies characteristics about the event listener. The available
    options are:
    <dl>
      <dt><code>capture</code></dt>
      <dd>A boolean value indicating that events of this type will be dispatched
        to the registered <code><var>listener</var></code> before being dispatched to any
        <code>EventTarget</code> beneath it in the DOM tree.</dd>
      <dt><code>once</code></dt>
      <dd>A boolean value indicating that the <code><var>listener</var></code>
        should be invoked at most once after being added. If <code>true</code>, the
        <code><var>listener</var></code> would be automatically removed when invoked.</dd>
      <dt><code>passive</code></dt>
      <dd>A boolean value that, if <code>true</code>, indicates that the function
        specified by <code><var>listener</var></code> will never call
        {{domxref("Event.preventDefault", "preventDefault()")}}. If a passive listener
        does call <code>preventDefault()</code>, the user agent will do nothing other than
        generate a console warning. See <a href="#improving_scrolling_performance_with_passive_listeners">Improving scrolling performance with
          passive listeners</a> to learn more.</dd>
      <dt><code>signal</code></dt>
      <dd>An {{domxref("AbortSignal")}}. The listener will be removed when the given <code>AbortSignal</code>’s {{domxref("AbortController/abort()", "abort()")}} method is called.</dd>
      <dt><code>mozSystemGroup</code> {{non-standard_inline}}</dt>
      <dd>A boolean value indicating that the listener should be added to the
        system group. Available only in code running in XBL or in the
        {{glossary("chrome")}} of the Firefox browser.</dd>
    </dl>
  </dd>
  <dt><code><var>useCapture</var></code> {{optional_inline}}</dt>
  <dd><p>A boolean value indicating whether events of this type will be dispatched to
    the registered <code><var>listener</var></code> <em>before</em> being dispatched to
    any <code>EventTarget</code> beneath it in the DOM tree. Events that are bubbling
    upward through the tree will not trigger a listener designated to use capture. Event
    bubbling and capturing are two ways of propagating events that occur in an element
    that is nested within another element, when both elements have registered a handle for
    that event. The event propagation mode determines the order in which elements receive
    the event. See <a href="https://www.w3.org/TR/DOM-Level-3-Events/#event-flow">DOM
      Level 3 Events</a> and <a
      href="https://www.quirksmode.org/js/events_order.html#link4">JavaScript Event
      order</a> for a detailed explanation. If not specified,
    <code><var>useCapture</var></code> defaults to <code>false</code>.</p>
    <div class="notecard note">
      <p><strong>Note:</strong> For event listeners attached to the event target, the event is in the target phase,
        rather than the capturing and bubbling phases.
        Event listeners in the “capturing” phase are called before event listeners in any non-capturing phases.</p>
    </div>
  </dd>
  <dt><code><var>wantsUntrusted</var></code> {{optional_inline}} {{Non-standard_inline}}</dt>
  <dd>A Firefox (Gecko)-specific parameter. If <code>true</code>, the listener receives
    synthetic events dispatched by web content (the default is <code>false</code> for
    browser {{glossary("chrome")}} and <code>true</code> for regular web pages). This
    parameter is useful for code found in add-ons, as well as the browser itself.</dd>
</dl>

<h3 id="Return_value">Return value</h3>

<p><code>undefined</code></p>

<h2 id="Usage_notes">Usage notes</h2>

<h3 id="The_event_listener_callback">The event listener callback</h3>

<p>The event listener can be specified as either a callback function or an object that
  implements {{domxref("EventListener")}}, whose {{domxref("EventListener.handleEvent()",
  "handleEvent()")}} method serves as the callback function.</p>

<p>The callback function itself has the same parameters and return value as the
  <code>handleEvent()</code> method; that is, the callback accepts a single parameter: an
  object based on {{domxref("Event")}} describing the event that has occurred, and it
  returns nothing.</p>

<p>For example, an event handler callback that can be used to handle both
  {{domxref("Element/fullscreenchange_event", "fullscreenchange")}} and
  {{domxref("Element/fullscreenerror_event", "fullscreenerror")}} might look like this:
</p>

<pre class="brush: js">function eventHandler(event) {
  if (event.type == 'fullscreenchange') {
    /* handle a full screen toggle */
  } else /* fullscreenerror */ {
    /* handle a full screen toggle error */
  }
}</pre>

<h3 id="Safely_detecting_option_support">Safely detecting option support</h3>

<p>In older versions of the DOM specification, the third parameter of
  <code>addEventListener()</code> was a Boolean value indicating whether or not to use
  capture. Over time, it became clear that more options were needed. Rather than adding
  more parameters to the function (complicating things enormously when dealing with
  optional values), the third parameter was changed to an object that can contain various
  properties defining the values of options to configure the process of removing the event
  listener.</p>

<p>Because older browsers (as well as some not-too-old browsers) still assume the third
  parameter is a Boolean, you need to build your code to handle this scenario
  intelligently. You can do this by using feature detection for each of the options you're
  interested in.</p>

<p>For example, if you want to check for the <code>passive</code> option:</p>

<pre class="brush: js">let passiveSupported = false;

try {
  const options = {
    get passive() { // This function will be called when the browser
                    //   attempts to access the passive property.
      passiveSupported = true;
      return false;
    }
  };

  window.addEventListener("test", null, options);
  window.removeEventListener("test", null, options);
} catch(err) {
  passiveSupported = false;
}
</pre>

<p>This creates an <code><var>options</var></code> object with a getter function for the
  <code>passive</code> property; the getter sets a flag,
  <code><var>passiveSupported</var></code>, to <code>true</code> if it gets called. That
  means that if the browser checks the value of the <code>passive</code> property on the
  <code><var>options</var></code> object, <code><var>passiveSupported</var></code> will be
  set to <code>true</code>; otherwise, it will remain <code>false</code>. We then call
  <code>addEventListener()</code> to set up a fake event handler, specifying those
  options, so that the options will be checked if the browser recognizes an object as the
  third parameter. Then, we call <code>removeEventListener()</code> to clean up after
  ourselves. (Note that <code>handleEvent()</code> is ignored on event listeners that
  aren't called.)</p>

<p>You can check whether any option is supported this way. Just add a getter for that
  option using code similar to what is shown above.</p>

<p>Then, when you want to create an actual event listener that uses the options in
  question, you can do something like this:</p>

<pre class="brush: js">someElement.addEventListener("mouseup", handleMouseUp, passiveSupported
                               ? { passive: true } : false);</pre>

<p>Here we're adding a listener for the {{domxref("Element/mouseup_event", "mouseup")}}
  event on the element <code><var>someElement</var></code>. For the third parameter, if
  <code><var>passiveSupported</var></code> is <code>true</code>, we're specifying an
  <code><var>options</var></code> object with <code>passive</code> set to
  <code>true</code>; otherwise, we know that we need to pass a Boolean, and we pass
  <code>false</code> as the value of the <code><var>useCapture</var></code> parameter.</p>

<p>If you'd prefer, you can use a third-party library like <a
    href="https://modernizr.com/docs">Modernizr</a> or <a
    href="https://github.com/rafrex/detect-it">Detect It</a> to do this test for you.</p>

<p>You can learn more from the article about
  <code><a href="https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md#feature-detection">EventListenerOptions</a></code>
  from the <a href="https://wicg.github.io/admin/charter.html">Web Incubator Community
    Group</a>.</p>

<h2 id="Examples">Examples</h2>

<h3 id="Add_a_simple_listener">Add a simple listener</h3>

<p>This example demonstrates how to use <code>addEventListener()</code> to watch for mouse
  clicks on an element.</p>

<h4 id="HTML">HTML</h4>

<pre class="brush: html">&lt;table id="outside"&gt;
  &lt;tr&gt;&lt;td id="t1"&gt;one&lt;/td&gt;&lt;/tr&gt;
  &lt;tr&gt;&lt;td id="t2"&gt;two&lt;/td&gt;&lt;/tr&gt;
&lt;/table&gt;
</pre>

<h4 id="JavaScript">JavaScript</h4>

<pre class="brush: js">// Function to change the content of t2
function modifyText() {
  const t2 = document.getElementById("t2");
  if (t2.firstChild.nodeValue == "three") {
    t2.firstChild.nodeValue = "two";
  } else {
    t2.firstChild.nodeValue = "three";
  }
}

// Add event listener to table
const el = document.getElementById("outside");
el.addEventListener("click", modifyText, false);
</pre>

<p>In this code, <code>modifyText()</code> is a listener for <code>click</code> events
  registered using <code>addEventListener()</code>. A click anywhere in the table bubbles
  up to the handler and runs <code>modifyText()</code>.</p>

<h4 id="Result">Result</h4>

<p>{{EmbedLiveSample('Add_a_simple_listener')}}</p>

<h3 id="Add_an_abortable_listener">Add an abortable listener</h3>

<p>This example demonstrates how to add an <code>addEventListener()</code> that can be aborted with an {{domxref("AbortSignal")}}.</p>

<h4 id="HTML_5">HTML</h4>

<pre class="brush: html">&lt;table id="outside"&gt;
  &lt;tr&gt;&lt;td id="t1"&gt;one&lt;/td&gt;&lt;/tr&gt;
  &lt;tr&gt;&lt;td id="t2"&gt;two&lt;/td&gt;&lt;/tr&gt;
&lt;/table&gt;
</pre>

<h4 id="JavaScript_5">JavaScript</h4>

<pre class="brush: js">// Add an abortable event listener to table
const controller = new AbortController();
const el = document.getElementById("outside");
el.addEventListener("click", modifyText, { signal: controller.signal } );

// Function to change the content of t2
function modifyText() {
  const t2 = document.getElementById("t2");
  if (t2.firstChild.nodeValue == "three") {
    t2.firstChild.nodeValue = "two";
  } else {
    t2.firstChild.nodeValue = "three";
    controller.abort(); // remove listener after value reaches "three"
  }
}

</pre>

<p>In the above example just above, we modify the code in the previous example such that after the second row’s content changes to "three", we call <code>abort()</code> from the {{domxref("AbortController")}} we passed to the <code>addEventListener()</code> call. That results in the value remaining as "three" forever — because we no longer have any code listening for a click event.</p>

<h4 id="Result_5">Result</h4>

<p>{{EmbedLiveSample('Add_an_abortable_listener')}}</p>

<h3 id="Event_listener_with_anonymous_function">Event listener with anonymous function
</h3>

<p>Here, we'll take a look at how to use an anonymous function to pass parameters into the
  event listener.</p>

<h4 id="HTML_2">HTML</h4>

<pre class="brush: html">&lt;table id="outside"&gt;
  &lt;tr&gt;&lt;td id="t1"&gt;one&lt;/td&gt;&lt;/tr&gt;
  &lt;tr&gt;&lt;td id="t2"&gt;two&lt;/td&gt;&lt;/tr&gt;
&lt;/table&gt;</pre>

<h4 id="JavaScript_2">JavaScript</h4>

<pre class="brush: js">// Function to change the content of t2
function modifyText(new_text) {
  const t2 = document.getElementById("t2");
  t2.firstChild.nodeValue = new_text;
}

// Function to add event listener to table
const el = document.getElementById("outside");
el.addEventListener("click", function(){modifyText("four")}, false);
</pre>

<p>Notice that the listener is an anonymous function that encapsulates code that is then,
  in turn, able to send parameters to the <code>modifyText()</code> function, which is
  responsible for actually responding to the event.</p>

<h4 id="Result_2">Result</h4>

<p>{{EmbedLiveSample('Event_listener_with_anonymous_function')}}</p>

<h3 id="Event_listener_with_an_arrow_function">Event listener with an arrow function</h3>

<p>This example demonstrates a simple event listener implemented using arrow function
  notation.</p>

<h4 id="HTML_3">HTML</h4>

<pre class="brush: html">&lt;table id="outside"&gt;
  &lt;tr&gt;&lt;td id="t1"&gt;one&lt;/td&gt;&lt;/tr&gt;
  &lt;tr&gt;&lt;td id="t2"&gt;two&lt;/td&gt;&lt;/tr&gt;
&lt;/table&gt;
</pre>

<h4 id="JavaScript_3">JavaScript</h4>

<pre class="brush: js">// Function to change the content of t2
function modifyText(new_text) {
  const t2 = document.getElementById("t2");
  t2.firstChild.nodeValue = new_text;
}

// Add event listener to table with an arrow function
const el = document.getElementById("outside");
el.addEventListener("click", () =&gt; { modifyText("four"); }, false);
</pre>

<h4 id="Result_3">Result</h4>

<p>{{EmbedLiveSample('Event_listener_with_an_arrow_function')}}</p>

<p>Please note that while anonymous and arrow functions are similar, they have different
  <code>this</code> bindings. While anonymous (and all traditional JavaScript functions)
  create their own <code>this</code> bindings, arrow functions inherit the
  <code>this</code> binding of the containing function.</p>

<p>That means that the variables and constants available to the containing function are
  also available to the event handler when using an arrow function.</p>

<h3 id="Example_of_options_usage">Example of options usage</h3>

<h4 id="HTML_4">HTML</h4>

<pre class="brush: html">&lt;div class="outer"&gt;
  outer, once &amp; none-once
  &lt;div class="middle" target="_blank"&gt;
    middle, capture &amp; none-capture
    &lt;a class="inner1" href="https://www.mozilla.org" target="_blank"&gt;
      inner1, passive &amp; preventDefault(which is not allowed)
    &lt;/a&gt;
    &lt;a class="inner2" href="https://developer.mozilla.org/" target="_blank"&gt;
      inner2, none-passive &amp; preventDefault(not open new page)
    &lt;/a&gt;
  &lt;/div&gt;
&lt;/div&gt;
</pre>

<h4 id="CSS">CSS</h4>

<pre class="brush: css">.outer, .middle, .inner1, .inner2 {
  display: block;
  width:   520px;
  padding: 15px;
  margin:  15px;
  text-decoration: none;
}
.outer {
  border: 1px solid red;
  color:  red;
}
.middle {
  border: 1px solid green;
  color:  green;
  width:  460px;
}
.inner1, .inner2 {
  border: 1px solid purple;
  color:  purple;
  width:  400px;
}
</pre>

<h4 id="JavaScript_4">JavaScript</h4>

<pre class="brush: js">const outer  = document.querySelector('.outer');
const middle = document.querySelector('.middle');
const inner1 = document.querySelector('.inner1');
const inner2 = document.querySelector('.inner2');

const capture = {
  capture : true
};
const noneCapture = {
  capture : false
};
const once = {
  once : true
};
const noneOnce = {
  once : false
};
const passive = {
  passive : true
};
const nonePassive = {
  passive : false
};

outer.addEventListener('click', onceHandler, once);
outer.addEventListener('click', noneOnceHandler, noneOnce);
middle.addEventListener('click', captureHandler, capture);
middle.addEventListener('click', noneCaptureHandler, noneCapture);
inner1.addEventListener('click', passiveHandler, passive);
inner2.addEventListener('click', nonePassiveHandler, nonePassive);

function onceHandler(event) {
  alert('outer, once');
}
function noneOnceHandler(event) {
  alert('outer, none-once, default');
}
function captureHandler(event) {
  //event.stopImmediatePropagation();
  alert('middle, capture');
}
function noneCaptureHandler(event) {
  alert('middle, none-capture, default');
}
function passiveHandler(event) {
  // Unable to preventDefault inside passive event listener invocation.
  event.preventDefault();
  alert('inner1, passive, open new page');
}
function nonePassiveHandler(event) {
  event.preventDefault();
  //event.stopPropagation();
  alert('inner2, none-passive, default, not open new page');
}
</pre>

<h4 id="Result_4">Result</h4>

<p>Click the outer, middle, inner containers respectively to see how the options work.</p>

<p>{{ EmbedLiveSample('Example_of_options_usage', 600, 310, '',
  'Web/API/EventTarget/addEventListener') }}</p>

<p>Before using a particular value in the <code><var>options</var></code> object, it's a
  good idea to ensure that the user's browser supports it, since these are an addition
  that not all browsers have supported historically. See {{anch("Safely detecting option
  support")}} for details.</p>

<h2 id="Other_notes">Other notes</h2>

<h3 id="Why_use_addEventListener">Why use addEventListener()?</h3>

<p><code>addEventListener()</code> is the way to register an event listener as specified
  in W3C DOM. The benefits are as follows:</p>

<ul>
  <li>It allows adding more than a single handler for an event. This is particularly
    useful for {{Glossary("AJAX")}} libraries, JavaScript modules, or any other kind of
    code that needs to work well with other libraries/extensions.</li>
  <li>It gives you finer-grained control of the phase when the listener is activated
    (capturing vs. bubbling).</li>
  <li>It works on any DOM element, not just HTML elements.</li>
</ul>

<p>The alternative, <a href="#older_way_to_register_event_listeners">older way to register
    event listeners</a>, is described below.</p>

<h3 id="Adding_a_listener_during_event_dispatch">Adding a listener during event dispatch
</h3>

<p>If an {{domxref("EventListener")}} is added to an {{domxref("EventTarget")}} while it
  is processing an event, that event does not trigger the listener. However, that same
  listener may be triggered during a later stage of event flow, such as the bubbling
  phase.</p>

<h3 id="Multiple_identical_event_listeners">Multiple identical event listeners</h3>

<p>If multiple identical <code>EventListener</code>s are registered on the same
  <code>EventTarget</code> with the same parameters, the duplicate instances are
  discarded. They do not cause the <code>EventListener</code> to be called twice, and they
  do not need to be removed manually with the
  {{domxref("EventTarget.removeEventListener()", "removeEventListener()")}} method.</p>

<p>Note, however that when using an anonymous function as the handler, such listeners will
  NOT be identical, because anonymous functions are not identical even if defined using
  the SAME unchanging source-code called repeatedly, even if in a loop.</p>

<p>However, repeatedly defining the same named function in such cases can be more
  problematic. (See <a href="#memory_issues">Memory issues</a>, below.)</p>

<h3 id="The_value_of_this_within_the_handler">The value of "this" within the handler</h3>

<p>It is often desirable to reference the element on which the event handler was fired,
  such as when using a generic handler for a set of similar elements.</p>

<p>When attaching a handler function to an element using <code>addEventListener()</code>,
  the value of {{jsxref("Operators/this","this")}} inside the handler will be a reference to
  the element. It will be the same as the value of the <code>currentTarget</code> property of
  the event argument that is passed to the handler.</p>

<pre class="brush: js">my_element.addEventListener('click', function (e) {
  console.log(this.className)           // logs the className of my_element
  console.log(e.currentTarget === this) // logs `true`
})
</pre>

<p>As a reminder, <a
    href="/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#no_separate_this">arrow
    functions do not have their own <code>this</code> context</a>.</p>

<pre class="brush: js">my_element.addEventListener('click', (e) =&gt; {
  console.log(this.className)           // WARNING: `this` is not `my_element`
  console.log(e.currentTarget === this) // logs `false`
})</pre>

<p>If an event handler (for example, {{domxref("GlobalEventHandlers.onclick",
  "onclick")}}) is specified on an element in the HTML source, the JavaScript code in the
  attribute value is effectively wrapped in a handler function that binds the value of
  <code>this</code> in a manner consistent with the <code>addEventListener()</code>; an
  occurrence of <code>this</code> within the code represents a reference to the element.
</p>

<pre class="brush: html">&lt;table id="my_table" onclick="console.log(this.id);"&gt;&lt;!-- `this` refers to the table; logs 'my_table' --&gt;
  ...
&lt;/table&gt;
</pre>

<p>Note that the value of <code>this</code> inside a function, <em>called by</em> the code
  in the attribute value, behaves as per <a
    href="/en-US/docs/Web/JavaScript/Reference/Operators/this">standard rules</a>. This is
  shown in the following example:</p>

<pre class="brush: html">&lt;script&gt;
  function logID() { console.log(this.id); }
&lt;/script&gt;
&lt;table id="my_table" onclick="logID();"&gt;&lt;!-- when called, `this` will refer to the global object --&gt;
  ...
&lt;/table&gt;
</pre>

<p>The value of <code>this</code> within <code>logID()</code> is a reference to the global
  object {{domxref("Window")}} (or <code>undefined</code> in the case of <a
    href="/en-US/docs/Web/JavaScript/Reference/Strict_mode">strict mode</a>.</p>

<h4 id="Specifying_this_using_bind">Specifying "this" using bind()</h4>

<p>The {{jsxref("Function.prototype.bind()")}} method lets you establish a fixed
  <code>this</code> context for all subsequent calls — bypassing problems where it's unclear what <code>this</code> will be, depending on
  the context from which your function was called. Note, however, that you'll need to keep
  a reference to the listener around so you can remove it later.</p>

<p>This is an example with and without <code>bind()</code>:</p>

<pre class="brush: js">const Something = function(element) {
  // |this| is a newly created object
  this.name = 'Something Good';
  this.onclick1 = function(event) {
    console.log(this.name); // undefined, as |this| is the element
  };

  this.onclick2 = function(event) {
    console.log(this.name); // 'Something Good', as |this| is bound to newly created object
  };

  // bind causes a fixed `this` context to be assigned to onclick2
  this.onclick2 = this.onclick2.bind(this);

  element.addEventListener('click', this.onclick1, false);
  element.addEventListener('click', this.onclick2, false); // Trick
}
const s = new Something(document.body);
</pre>

<p>Another solution is using a special function called <code>handleEvent()</code> to catch
  any events:</p>

<pre class="brush: js">const Something = function(element) {
  // |this| is a newly created object
  this.name = 'Something Good';
  this.handleEvent = function(event) {
    console.log(this.name); // 'Something Good', as this is bound to newly created object
    switch(event.type) {
      case 'click':
        // some code here...
        break;
      case 'dblclick':
        // some code here...
        break;
    }
  };

  // Note that the listeners in this case are |this|, not this.handleEvent
  element.addEventListener('click', this, false);
  element.addEventListener('dblclick', this, false);

  // You can properly remove the listeners
  element.removeEventListener('click', this, false);
  element.removeEventListener('dblclick', this, false);
}
const s = new Something(document.body);
</pre>

<p>Another way of handling the reference to <em>this</em> is to pass to the
  <code>EventListener</code> a function that calls the method of the object that contains
  the fields that need to be accessed:</p>

<pre class="brush: js">class SomeClass {

  constructor() {
    this.name = 'Something Good';
  }

  register() {
    const that = this;
    window.addEventListener('keydown', function(e) { that.someMethod(e); });
  }

  someMethod(e) {
    console.log(this.name);
    switch(e.keyCode) {
      case 5:
        // some code here...
        break;
      case 6:
        // some code here...
        break;
    }
  }

}

const myObject = new SomeClass();
myObject.register();</pre>

<h3 id="Getting_data_into_and_out_of_an_event_listener">Getting data into and out of an
  event listener</h3>

<p>It may seem that event listeners are like islands, and that it is extremely difficult
  to pass them any data, much less to get any data back from them after they execute.
  Event listeners only take one argument, the <a
    href="/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_objects">Event
    Object</a>, which is automatically passed to the listener, and the return value is
  ignored. So how can we get data in and back out of them again? There are a number of
  good methods for doing this.</p>

<h4 id="Getting_data_into_an_event_listener_using_this">Getting data into an event
  listener using "this"</h4>

<p>As mentioned <a href="#specifying_this_using_bind">above</a>, you can use
  <code>Function.prototype.bind()</code> to pass a value to an event listener via the
  <code>this</code> reference variable.</p>

<pre class="brush: js">const myButton = document.getElementById('my-button-id');
const someString = 'Data';

myButton.addEventListener('click', function () {
  console.log(this);  // Expected Value: 'Data'
}.bind(someString));
</pre>

<p>This method is suitable when you don't need to know which HTML element the event
  listener fired on programmatically from within the event listener. The primary benefit
  to doing this is that the event listener receives the data in much the same way that it
  would if you were to actually pass it through its argument list.</p>

<h4 id="Getting_data_into_an_event_listener_using_the_outer_scope_property">Getting data
  into an event listener using the outer scope property</h4>

<p>When an outer scope contains a variable declaration (with <code>const</code>,
  <code>let</code>), all the inner functions declared in that scope have access to that
  variable (look <a
    href="/en-US/docs/Glossary/Function#different_types_of_functions">here</a> for
  information on outer/inner functions, and <a
    href="/en-US/docs/Web/JavaScript/Reference/Statements/var#implicit_globals_and_outer_function_scope">here</a>
  for information on variable scope). Therefore, one of the simplest ways to access data
  from outside of an event listener is to make it accessible to the scope in which the
  event listener is declared.</p>

<pre class="brush: js">const myButton = document.getElementById('my-button-id');
let someString = 'Data';

myButton.addEventListener('click', function() {
  console.log(someString);  // Expected Value: 'Data'

  someString = 'Data Again';
});

console.log(someString);  // Expected Value: 'Data' (will never output 'Data Again')
</pre>

<div class="notecard note">
  <p><strong>Note:</strong> Although inner scopes have access to <code>const</code>,
    <code>let</code> variables from outer scopes, you cannot expect any changes to these
    variables to be accessible after the event listener definition, within the same outer
    scope. Why? Because by the time the event listener would execute, the scope in which
    it was defined would have already finished executing.</p>
</div>

<h4 id="Getting_data_into_and_out_of_an_event_listener_using_objects">Getting data into
  and out of an event listener using objects</h4>

<p>Unlike most functions in JavaScript, objects are retained in memory as long as a
  variable referencing them exists in memory. This, and the fact that objects can have
  properties, and that they can be passed around by reference, makes them likely
  candidates for sharing data among scopes. Let's explore this.</p>

  <div class="notecard note">
  <p><strong>Note:</strong> Functions in JavaScript are actually objects. (Hence they too
    can have properties, and will be retained in memory even after they finish executing
    if assigned to a variable that persists in memory.)</p>
</div>

<p>Because object properties can be used to store data in memory as long as a variable
  referencing the object exists in memory, you can actually use them to get data into an
  event listener, and any changes to the data back out after an event handler executes.
  Consider this example.</p>

<pre class="brush: js">const myButton = document.getElementById('my-button-id');
const someObject = {aProperty: 'Data'};

myButton.addEventListener('click', function() {
  console.log(someObject.aProperty);  // Expected Value: 'Data'

  someObject.aProperty = 'Data Again';  // Change the value
});

window.setInterval(function() {
  if (someObject.aProperty === 'Data Again') {
    console.log('Data Again: True');
    someObject.aProperty = 'Data';  // Reset value to wait for next event execution
  }
}, 5000);
</pre>

<p>In this example, even though the scope in which both the event listener and the
  interval function are defined would have finished executing before the original value of
  <code>someObject.aProperty</code> would have changed, because <code>someObject</code>
  persists in memory (by <em>reference</em>) in both the event listener and interval
  function, both have access to the same data (i.e. when one changes the data, the other
  can respond to the change).</p>

  <div class="notecard note">
    <p><strong>Note:</strong> Objects are stored in variables by reference, meaning only the
    memory location of the actual data is stored in the variable. Among other things, this
    means variables that "store" objects can actually affect other variables that get
    assigned ("store") the same object reference. When two variables reference the same
    object (e.g., <code>let a = b = {aProperty: 'Yeah'};</code>), changing the data in
    either variable will affect the other.</p>
</div>

<div class="notecard note">
  <p><strong>Note:</strong> Because objects are stored in variables by reference, you can
    return an object from a function to keep it alive (preserve it in memory so you don't
    lose the data) after that function stops executing.</p>
</div>

<h3 id="Legacy_Internet_Explorer_and_attachEvent">Legacy Internet Explorer and attachEvent
</h3>

<p>In Internet Explorer versions before IE 9, you have to use
  <code>attachEvent()</code>, rather than the standard
  <code>addEventListener()</code>. For IE, we modify the preceding example to:</p>

<pre class="brush: js">if (el.addEventListener) {
  el.addEventListener('click', modifyText, false);
} else if (el.attachEvent)  {
  el.attachEvent('onclick', modifyText);
}
</pre>

<p>There is a drawback to <code>attachEvent()</code>: The value of <code>this</code> will
  be a reference to the <code>window</code> object, instead of the element on which it was
  fired.</p>

<p>The <code>attachEvent()</code> method could be paired with the <code>onresize</code>
  event to detect when certain elements in a web page were resized. The proprietary
  <code>mselementresize</code> event, when paired with the <code>addEventListener</code>
  method of registering event handlers, provides similar functionality as
  <code>onresize</code>, firing when certain HTML elements are resized.</p>

<h3 id="Polyfill">Polyfill</h3>

<p>You can work around <code>addEventListener()</code>,
  <code>removeEventListener()</code>, {{domxref("Event.preventDefault()")}}, and
  {{domxref("Event.stopPropagation()")}} not being supported by Internet Explorer 8 by
  using the following code at the beginning of your script. The code supports the use of
  <code>handleEvent()</code> and also the {{event("DOMContentLoaded")}} event.</p>

  <div class="notecard note">
    <p><strong>Note:</strong> <code>useCapture</code> is not supported, as IE 8 does not
    have any alternative method. The following code only adds IE 8 support. This IE 8
    polyfill only works in standards mode: a doctype declaration is required.</p>
</div>

<pre class="brush: js">(function() {
  if (!Event.prototype.preventDefault) {
    Event.prototype.preventDefault=function() {
      this.returnValue=false;
    };
  }
  if (!Event.prototype.stopPropagation) {
    Event.prototype.stopPropagation=function() {
      this.cancelBubble=true;
    };
  }
  if (!Element.prototype.addEventListener) {
    var eventListeners=[];

    var addEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var self=this;
      var wrapper=function(e) {
        e.target=e.srcElement;
        e.currentTarget=self;
        if (typeof listener.handleEvent != 'undefined') {
          listener.handleEvent(e);
        } else {
          listener.call(self,e);
        }
      };
      if (type=="DOMContentLoaded") {
        var wrapper2=function(e) {
          if (document.readyState=="complete") {
            wrapper(e);
          }
        };
        document.attachEvent("onreadystatechange",wrapper2);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper2});

        if (document.readyState=="complete") {
          var e=new Event();
          e.srcElement=window;
          wrapper2(e);
        }
      } else {
        this.attachEvent("on"+type,wrapper);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper});
      }
    };
    var removeEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var counter=0;
      while (counter&lt;eventListeners.length) {
        var eventListener=eventListeners[counter];
        if (eventListener.object==this &amp;&amp; eventListener.type==type &amp;&amp; eventListener.listener==listener) {
          if (type=="DOMContentLoaded") {
            this.detachEvent("onreadystatechange",eventListener.wrapper);
          } else {
            this.detachEvent("on"+type,eventListener.wrapper);
          }
          eventListeners.splice(counter, 1);
          break;
        }
        ++counter;
      }
    };
    Element.prototype.addEventListener=addEventListener;
    Element.prototype.removeEventListener=removeEventListener;
    if (HTMLDocument) {
      HTMLDocument.prototype.addEventListener=addEventListener;
      HTMLDocument.prototype.removeEventListener=removeEventListener;
    }
    if (Window) {
      Window.prototype.addEventListener=addEventListener;
      Window.prototype.removeEventListener=removeEventListener;
    }
  }
})();</pre>

<h3 id="Older_way_to_register_event_listeners">Older way to register event listeners</h3>

<p><code>addEventListener()</code> was introduced with the DOM 2 <a
    href="https://www.w3.org/TR/DOM-Level-2-Events">Events</a> specification. Before then,
  event listeners were registered as follows:</p>

<pre class="brush: js">// Passing a function reference — do not add '()' after it, which would call the function!
el.onclick = modifyText;

// Using a function expression
element.onclick = function() {
  // ... function logic ...
};
</pre>

<p>This method replaces the existing <code>click</code> event listener(s) on the element
  if there are any. Other events and associated event handlers such as <code>blur</code>
  (<code>onblur</code>) and <code>keypress</code> (<code>onkeypress</code>) behave
  similarly.</p>

<p>Because it was essentially part of {{glossary("DOM", "DOM 0")}}, this technique for
  adding event listeners is very widely supported and requires no special cross-browser
  code. It is used to register event listeners dynamically when very old browsers (like IE
  &lt;=8) must be supported; see the table below for details on browser support for
  <code>addEventListener</code>.</p>

<h3 id="Memory_issues">Memory issues</h3>

<pre class="brush: js">const els = document.getElementsByTagName('*');

// Case 1
for(let i=0 ; i &lt; els.length; i++){
  els[i].addEventListener("click", function(e){/*do something*/}, false);
}

// Case 2
function processEvent(e){
  /* do something */
}

for(let i=0 ; i &lt; els.length; i++){
  els[i].addEventListener("click", processEvent, false);
}
</pre>

<p>In the first case above, a new (anonymous) handler function is created with each
  iteration of the loop. In the second case, the same previously declared function is used
  as an event handler, which results in smaller memory consumption because there is only
  one handler function created. Moreover, in the first case, it is not possible to call
  {{domxref("EventTarget.removeEventListener", "removeEventListener()")}} because no
  reference to the anonymous function is kept (or here, not kept to any of the multiple
  anonymous functions the loop might create.) In the second case, it's possible to do
  <code><var>myElement</var>.removeEventListener("click", processEvent, false)</code>
  because <code>processEvent</code> is the function reference.</p>

<p>Actually, regarding memory consumption, the lack of keeping a function reference is not
  the real issue; rather it is the lack of keeping a STATIC function reference. In both
  problem-cases below, a function reference is kept, but since it is redefined on each
  iteration, it is not static. In the third case, the reference to the anonymous function
  is being reassigned with each iteration. In the fourth case, the entire function
  definition is unchanging, but it is still being repeatedly defined as if new (unless it
  was [[promoted]] by the compiler) and so is not static. Therefore, though appearing to
  be [[Multiple identical event listeners]], in both cases each iteration will instead
  create a new listener with its own unique reference to the handler function. However,
  since the function definition itself does not change, the SAME function may still be
  called for every duplicate listener (especially if the code gets optimized.)</p>

<p>Also in both cases, because the function reference was kept but repeatedly redefined
  with each add, the remove-statement from above can still remove a listener, but now only
  the last one added.</p>

<pre class="brush: js">// For illustration only: Note "MISTAKE" of [j] for [i] thus causing desired events to all attach to SAME element

// Case 3
for(let i=0, j=0 ; i&lt;els.length ; i++){
  /* do lots of stuff with j */
  els[j].addEventListener("click", processEvent = function(e){/*do something*/}, false);
}

// Case 4
for(let i=0, j=0 ; i&lt;els.length ; i++){
  /* do lots of stuff with j */
  function processEvent(e){/*do something*/};
  els[j].addEventListener("click", processEvent, false);
}</pre>

<h3 id="Improving_scrolling_performance_with_passive_listeners">Improving scrolling
  performance with passive listeners</h3>

<p>According to the specification, the default value for the <code>passive</code> option
  is always <code>false</code>. However, this introduces the potential for event listeners
  handling certain touch events (among others) to block the browser's main thread while it
  is attempting to handle scrolling, resulting in possibly enormous reduction in
  performance during scroll handling.</p>

<p>To prevent this problem, some browsers (specifically, Chrome and Firefox) have changed
  the default value of the <code>passive</code> option to <code>true</code> for the
  {{event("touchstart")}} and {{event("touchmove")}} events on the document-level nodes
  {{domxref("Window")}}, {{domxref("Document")}}, and {{domxref("Document.body")}}. This
  prevents the event listener from being called, so it can't block page rendering while
  the user is scrolling.</p>

  <div class="notecard note">
    <p><strong>Note:</strong> See the compatibility table below if you need to know which
    browsers (and/or which versions of those browsers) implement this altered behavior.</p>
  </div>

<p>You can override this behavior by explicitly setting the value of <code>passive</code>
  to <code>false</code>, as shown here:</p>

<pre class="brush: js">/* Feature detection */
let passiveIfSupported = false;

try {
  window.addEventListener("test", null,
    Object.defineProperty(
      {},
      "passive",
      {
        get: function() { passiveIfSupported = { passive: true }; }
      }
    )
  );
} catch(err) {}

window.addEventListener('scroll', function(event) {
  /* do something */
  // can't use event.preventDefault();
}, passiveIfSupported );
</pre>

<p>On older browsers that don't support the <code>options</code> parameter to
  <code>addEventListener()</code>, attempting to use it prevents the use of the
  <code>useCapture</code> argument without proper use of <a
    href="#safely_detecting_option_support">feature detection</a>.</p>

<p>You don't need to worry about the value of <code>passive</code> for the basic
  {{event("scroll")}} event. Since it can't be canceled, event listeners can't block page
  rendering anyway.</p>

<h2 id="Specifications">Specifications</h2>

{{Specifications}}

<h2 id="Browser_compatibility">Browser compatibility</h2>

<p>{{Compat}}</p>

<h2 id="See_also">See also</h2>

<ul>
  <li>{{domxref("EventTarget.removeEventListener()")}}</li>
  <li><a href="/en-US/docs/Web/Events/Creating_and_triggering_events">Creating and triggering custom events</a></li>
  <li><a href="https://www.quirksmode.org/js/this.html">More details on the use of <code>this</code> in event handlers</a></li>
</ul>