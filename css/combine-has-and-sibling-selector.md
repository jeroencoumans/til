# You can combine :has and ~ selectors

I was creating the CSS for a tabbed interface. The markup would look like this:

```html
<nav class="button-pills">
  <a class="button" href="#tab-1">Tab 1</a>
  <a class="button" href="#tab-2">Tab 2</a>
  <a class="button" href="#tab-3">Tab 3</a>
</nav>
<article id="tab-1">

</article>
<article id="tab-2">

</article>
<article id="tab-3">

</article>

```

For the tabs, I reused existing button pill styles to display the links as buttons next to each other, where the first and last buttons have rounded corners. The CSS is very simple:

```css
.button-pills > .button {
  border: 1px solid var(--grey);
  border-radius: 0;

  /* ...more styles snipped for brevity */
}

.button-pills > .button:first-child {
  border-radius: var(--border-radius) 0 0 var(--border-radius);
}

.button-pills > .button:last-child {
  border-radius: 0 var(--border-radius) var(--border-radius) 0;
}

```

I needed a selector that would match the pill-buttons, but only if they were directly followed by a card. This turned out to be very easy:

```css
.button-pills:has( ~ .card) > .button:first-child {
  border-radius: var(--size-extra-small) 0 0;
}

.button-pills:has( ~ .card) > .button:last-child {
  border-radius: 0 var(--size-extra-small) 0 0;
}
```

There are numerous other ways to achieve the same effect, but this got the job done quite easily!