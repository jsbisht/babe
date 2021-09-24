### Cropping part of an image

Apply height and width while giving object-fit as cover

```
img {
  height: 100px;
  width: 100px;
  object-fit: cover;
}
```

### Remove horizontal scroll

Get width without scrollbar

```
const { clientWidth: cw } = document.body;
```

### scss min() usage error

Use unquote to avoid conflict with scss min function

```scss
unquote('min(100px, 100vh)')
```

Variable in unquote string

```scss
'min(calc(33vw - (#{$card-margin} * 2)), 140px)'
```

### scss: Incompatible units: 'vw' and 'px'

CSS didn't use to have its own runtime min() and max() functions, and before they existed SASS had a compile-time version. Since SASS doesn't run live it would be impossible for it to determine whether 10vw or 40px is larger - hence the error.

Since SASS is case sensitive while CSS isn't, you can force the parser to use the CSS version of min or max by just calling MIN() or MAX() instead. 

## References

- Detect a touch device with css only (https://ferie.medium.com/detect-a-touch-device-with-only-css-9f8e30fa1134)
- Using media queries (https://css-tricks.com/approaches-media-queries-sass/)
- Light and Dark theme (https://www.pullrequest.com/blog/how-to-implement-dark-mode-with-css-js/)
