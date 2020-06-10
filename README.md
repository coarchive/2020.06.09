# 2020.06.09

*Did I just do three days worth of work for no reason?*

I solved a problem that didn't exist.
Instead of walking away from this in shame, I'm going to reflect on it so I
feel like my time was at least marginally worthwhile.

Introducing:

- [browser-zoom-listener](https://github.com/coarchive/browser-zoom-listener) referred to as BZL
- [simple-resize-observer](https://github.com/coarchive/simple-resize-observer) referred to as SRO

I read a StackOverflow answer that talked about the issues of too many events
being sent. I saw that the author used
[`window.requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)
and a dirty flag to debounce excess events so that the callback wasn't called
more than it needed to be. I thought it was so clever that I decided to make BZL.
BZL was more of a fight against TypeScript and it's exports.
I learned a whole lot about the type system so it wasn't entirely pointless.
So I finished BZL and published it... a few times because I screwed up.
It was some of the best code I had done in a hot minute and I was pleased.

SRO was basically a clone of BZL except it was for the `ResizeObserver`.
I hear someone saying that I should have made an abstraction over the two but
> *"I've made enough bad code choices to know when to stop abstracting."*
> (and other jokes I tell myself)

I polished up the demo. It even had HLJS!
I figured that I'd add a callback counter display for good measure.
And yet again, I had forgotten to set the dirty flag.
So I set the dirty flag and didn't notice a difference.
I was wondering if my code wasn't debouncing correctly.
After like two minutes of print debugging, the only conclusion that I could
come to was that the animation frames were dispatching events faster than I
could trip the debouncing mechanism.

[https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)
> `ResizeObserver` avoids infinite callback loops and cyclic dependencies that are often created when resizing via a callback function.
> It does this by only processing elements deeper in the DOM in subsequent frames.
> Implementations should, if they follow the specification, **invoke resize events before paint** and after layout.

Aaaaand the `ResizeObserver` invokes events only before paint.
Nice one! I solved a problem that didn't exist.
