Landing pages often have moving pictures to make them more interesting.
But making these movements with just HTML and CSS can be hard, so we use
special JavaScript libraries for animation.

For our <a href="https://marmelab.com/ra-enterprise/">react-admin</a> product&apos;s
landing page, we chose to use **GSAP**. We learned a lot about both the
GSAP library and animations in general during this process, and we also
encountered some tricky parts.

So, we decided to share what we learned in a series of articles:

1.  GSAP Basics: Dive Into Web Animations *(this post)*

2.  <a href="https://marmelab.com/blog/2024/04/11/trigger-animations-on-scroll-with-gsap-scrolltrigger.html">
  Trigger Animations On Scroll With GSAP<a/>

3.  <a href="https://marmelab.com/blog/2024/05/30/gsap-in-practice-avoid-the-pitfalls.html">
  GSAP in practice: some pitfalls to avoid</a>

Let&apos;s get started with a quick overview of GSAP.

**What is GSAP?**

<a href="https://gsap.com/">GSAP</a> (for GreenSock Animation Platform) is a
JavaScript library that allows to animate elements in a webpage. It
makes it possible to create complex and performant animations, while yet
offering a very friendly and versatile API.

It also comes with a variety of plugins, some of which are paid,
offering powerful and customizable tools to help animate almost
anything, from text to SVG, and even include some effects based on
physics.

Just have a look at their <a href="https://gsap.com/">homepage</a> to get a glimpse
of what this library can do, it&apos;s amazing!

Now that you are, hopefully, hyped up just like I am, let&apos;s dive in!

**Main Concepts**

In this section, I&apos;ll cover two of the most basic concepts introduced
by the library.

**Tween**

Probably the first concept you will come across when reading the GSAP
docs are **tweens**. The term tween comes from the word *between*, as a
tween is an object describing an animation *between* a **from** state
and a **to** state. It also holds the **target** (the object to
animate), and any other properties describing the animation, like its
duration, or the easing function used to calculate mid-animation values.

**Timeline**

Basically, the **timeline** answers the following question: how can I
trigger my animations in sequence, or relative to one another, without
having to calculate all the *delays* myself? The solution is to use a
timeline to group the tweens together, optionally specifying where the
tweens should be placed in the timeline with the position parameter. The
timeline can then be manipulated (play, pause, seek a specific frame,
&hellip;) as a whole without having to manage the playhead for all tweens
manually.

PLAYHEAD

<pre>
&vert;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;-timeline&dash;&dash;&dash;&dash;-&vert;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;-&vert;
&vert;&dash;-tween1&dash;-&vert; &vert;
&vert;&dash;&dash;&dash;&dash;-tween2&dash;&dash;&dash;&dash;-&vert;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;&dash;-&vert;
</pre>

**Syntax**

In this section, I will go through some of the code syntaxes that I
found most useful. But be sure to check out the <a href="https://gsap.com/resources/get-started">GSAP
docs</a> for more features and detailed examples.

**gsap.to()**

To create a tween, it can be as simple as:

<pre>
gsap.to(&quot;.box&quot;, { x: 100 });
</pre>

This tells GSAP to create a tween, **targeting** DOM elements matching
the CSS selector &quot;.box&quot;. The elements will be animated **from** their
current state (hence their current position in our example), **to** the
state { x: 100 }.

**Tip:** x is a <a href="https://gsap.com/resources/get-started/#transform-shorthand">
transform shorthand</a> provided by GSAP, equivalent to transform: translateX(100px).

https://codepen.io/slax57/pen/vYMBPGV
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 01.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image001.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- (./images/image001.png){width="6.5in" height="2.7263888888888888in"} -->

https://codepen.io/slax57/pen/vYMBPGV

We did not provide a duration, so by default the animation will take 0.5
seconds.

This can be easily changed using the duration <a href="https://gsap.com/resources/get-started#special-properties">
special property</a>:

<pre>
gsap.to(&quot;.box&quot;, { x: 100, duration: 2 });
</pre>

Also, our animation will run immediately, as soon as the page&apos;s
JavaScript is loaded. We can change that using the delay property:

<pre>
gsap.to(&quot;.box&quot;, { x: 100, duration: 2, delay: 1 });
</pre>

We can also change the &quot;feel&quot; of the animation by changing
the ease function. By default, the tween will use
the &quot;power1.out&quot; function. There are plenty to choose from, and GSAP
even offers a <a href="https://gsap.com/docs/v3/Eases/">tool</a> to better
visualize them. To make the animation a little more fun, we can for
instance use the &quot;elastic&quot; ease function:

<pre>
gsap.to(&quot;.box&quot;, { x: 100, duration: 2, delay: 1, ease: &quot;elastic&quot; });
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 02.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image002.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- (./images/image002.png){width="6.5in" height="2.717361111111111in"} -->

https://codepen.io/slax57/pen/KKYPEgV

**gsap.from()**

The gsap.to() function has a twin (not to be confused with tween! 😁️)
function: gsap.from(). It works exactly the same way. The only
difference is that, instead of animating from the element&apos;s current
state **to** the specified state, it will do the opposite.

The following example will animate elements with CSS
class &quot;.box&quot; from x: -100 to x: 0.

<pre>
gsap.from(&quot;.box&quot;, { x: -100 });
</pre>

This can be very useful to create animations that run on page load, or
to make new elements appear.

Here is another example with some opacity change, to make the element
fade in:

<pre>
gsap.from(&quot;.box&quot;, { x: -100, autoAlpha: 0 });
</pre>

**Tip:** autoAlpha is another <a href="https://gsap.com/resources/get-started/#transform-shorthand">
transform shorthand</a>), for both opacity and visibility.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 03.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image003.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- (./images/image003.png){width="6.5in" height="2.717361111111111in"} -->

https://codepen.io/slax57/pen/qBwWvqo

**gsap.fromTo()**

Now that we&apos;ve covered the gsap.to() and gsap.from() functions, you can
easily guess what the gsap.fromTo() function does. It can be useful in
certain cases to specify both a **from** and a **to** state, for
example, to apply a style as soon as the JavaScript is loaded, and
animate from it immediately after.

In the following example, if the animation is enabled, the element will
first have its opacity set to 0, and then will progressively reach an
opacity of 0.8. It the animation is disabled, the element will keep its
opacity of 1.

<pre>
if (shouldAnimate) {
  gsap.fromTo(&quot;.box&quot;, { opacity: 0 }, { opacity: 0.8 });
}
</pre>

**gsap.set()**

gsap.set() is the last function returning a *tween* that I&apos;d like to
cover. In essence, it&apos;s equivalent to calling gsap.to() with a duration
of 0: it allows to apply a state immediately to an element. This can be
useful to set a property only if a condition is met, or to set the state
from which we will be animating later.

<pre>
gsap.set(&quot;.box&quot;, { transformOrigin: &quot;center&quot; });
gsap.to(&quot;.box&quot;, { rotation: 360, repeat: -1, duration: 5, ease: &quot;linear&quot; });
</pre>

**Tip:** [repeat: -1](https://marmelab.com/blog/2024/03/27/infinitely) makes an animation
repeat infinitely.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 04.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image004.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- (./images/image004.png){width="6.5in" height="2.717361111111111in"} -->

https://codepen.io/slax57/pen/ExJYMWw

**gsap.timeline()**

As the name suggests, gsap.timeline() allows to create a **timeline**.

In the following example, the &quot;.circle&quot; element will start animate
right after the &quot;.logo&quot; completes its animation. This allows to run
the animations in sequence.

<pre>
const tl = gsap.timeline();
tl.from(&quot;.logo&quot;, { duration: 2.5, opacity: 0, scale: 0.3 }); // Instead of gsap.from()

tl.from(&quot;.circle&quot;, { duration: 1, opacity: 0, y: 150 }); // Instead of gsap.from()
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 05.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image005.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- (./images/image005.png){width="6.5in" height="2.7083333333333335in"} -->

https://codepen.io/slax57/pen/BaEBbYa

The main advantage of this syntax is that I can now change the duration
of the animation of &quot;.logo&quot;, and it will automatically keep the
animation of &quot;.circle&quot; in sync: it will always run as soon as
the &quot;.logo&quot; animation is complete.

Using a timeline also adds a new ability to
the to(), from(), fromTo() and set() functions: they now accept an
additional parameter, allowing to specify *where* in the timeline each
animation should be inserted.

<pre>
const tl = gsap.timeline();
tl.from(&quot;.logo&quot;, { duration: 2.5, opacity: 0, scale: 0.3 });

tl.from(&quot;.circle&quot;, { duration: 1, opacity: 0, y: 150 }, &quot;+=2&quot;); //
</pre>

Starts this animation 2 seconds past the end of the timeline (creates a gap)
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~ 06.  (xx) ~~~~~~~~~~~~~~~~~~~-->
<p align="left">
<img src="./images/image006.png"
  title=""
  alt="."
  style="width:6.5in;" />
</p>
<!-- ./images/image006.png){width="6.5in" height="2.6993055555555556in"} -->

https://codepen.io/slax57/pen/YzMKMGz

This extra parameter is called the <a href="https://gsap.com/docs/v3/GSAP/Timeline#positioning-animations-in-a-timeline">position parameter</a>,
and accepts many syntaxes to accommodate for all use cases:

-   3 insert at exactly 3 seconds from the start of the timeline

-   &quot;&lt;&quot; insert at the *start* of the previous animation

-   &quot;&gt;&quot; insert at the *end* of the previous animation

-   &quot;&lt;+=3&quot; 3 seconds past the start of the previous animation

-   &quot;&gt;-0.5&quot; 0.5 seconds before the end of the previous animation

-   &quot;-=25%&quot; overlap with the end of the timeline by 25% of the
    inserting animation&apos;s total duration

-   and so on&hellip;

This makes it so much easier to decompose a complex animation into small
steps and experiment with the timing of each step independently.

**Tip:** Timelines can also be nested.

**gsap.utils.toArray()**

This utility method allows to get an array of elements matching a
selector.

It can be useful to:

-   Know how many elements there are (for calculations)

-   Attach an animation to the specific element in the array rather than
    to all elements (useful to select the current element as a trigger
    for instance)

In the following example, we use this feature to set a different initial
rotation angle for each rectangle, and also a different duration,
resulting in different rotation speeds:

<pre>
const svgRectangles = gsap.utils.toArray(&apos;#my-svg rect&apos;);
svgRectangles.forEach((rect, i) =&gt; {
  gsap.set(rect, {
    rotation: i &ast; 15,
    transformOrigin: &apos;50% 50%&apos;,
  });
  gsap.to(rect, {
    rotation: 360 + i &ast; 15,
    duration: 100 - i &ast; 20,
    repeat: -1,
    ease: &apos;linear&apos;,
  });
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~ 07.  (xx) ~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p align="left"
<img src="./images/image007.png"
  loading="lazy"
  title=""
  alt="."
  style="width:6.5in height:2.717in;" />
<!-- ./images/image007.png){width="6.5in" height="2.717361111111111in"} -->

https://codepen.io/slax57/pen/OJGLGmb

**Conclusion**

This covers some of the basics of GSAP. With this knowledge, you should
already have enough to start creating your own animations.

I&apos;m overall pretty impressed with GSAP. In particular, I like its API,
which is very versatile, allowing for a very synthetic syntax for the
most common use cases, while yet allowing for advanced customization for
the most complex ones.

In the next post of this series, we will push complexity a little
further, by creating animations that trigger on page scroll.
