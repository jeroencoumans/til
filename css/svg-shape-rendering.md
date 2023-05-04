When rendering charts with SVG, you typically want to display them with crisp edges as follows:

```css
svg {
  shape-rendering: crispEdges;
}
```

However, when rendering images through SVG, such as a logo with custom text set as paths, you'll want to enable anti-aliasing so the text displays smoothly:

```css
.app-logo .logo-text {
  fill: currentColor;
  shape-rendering: geometricPrecision;
}
```

See here for details: https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/shape-rendering
