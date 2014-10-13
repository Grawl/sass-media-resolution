# media-resolution.sass

### Media Queries Resolution feature Sass polyfill

If you want to engage your stylesheet with “retina-ready” images (actually `hdpi` or `xhdpi`) with `@2x` size for example you need to wrap your style with a huge `@media` query:

```css
@media print,
	(-webkit-min-device-pixel-ratio: 1.5), // For WebKit/Blink
	(min--moz-device-pixel-ratio: 1.5), // For old Firefox
	(-o-min-device-pixel-ratio: 3 / 2), // For old Opera
	(min-device-pixel-ratio: 1.5), // Old unprefixed syntax
	(min-resolution: 144dpi), // Actual
	(min-resolution: 1.5dppx) { // Future
    .image {
      background-image: url("image@2x.png");
      }
```

Looking bad, do you?

I suggest a better way to do this: for anything you do with stylesheets there is a **Sass mixin!** *Or Ruby Sass extension, if you are CSS geek.*

## Using

```sass
+screen-xhdpi
	background-image: url('image@2x.png')
```

Will return you this CSS:

```css
@media print, (-webkit-min-device-pixel-ratio: 1.5), (min--moz-device-pixel-ratio: 1.5), (-o-min-device-pixel-ratio: 3 / 2), (min-device-pixel-ratio: 1.5), (min-resolution: 144dpi), (min-resolution: 1.5dppx) {
	.image {
		background-image: url("image@2x.png"); }
```

Remember that `xhdpi` is like Retina in Android terminology and equals to `2dppx`/`144dpi`.

### Or you can use it harder:

```sass
+max-resolution(2)
	.image
		background-image: url('image@2x.png')
```

### Or even harder with loops:

```sass
@each $prefix, $dppx in ((max, 1), (min, 1.1))
    .image
        +resolution($prefix, $dppx)
            $dppx: ceil($dppx)
            background-image: url('image@'+$dppx+'x.png')
```

See `tests/` for more information about how it works.

## Install via Bower

```bash
bower install --save sass-media-resolution
```

Import the Sass file:

```sass
@import bower_components/sass-media-resolution/media-resolution.sass
```

Or use your favorite build system to do this. Like [klei/grunt-injector](https://github.com/klei/grunt-injector), see how it works at [larsonjj/generator-yeogurt](https://github.com/larsonjj/generator-yeogurt/blob/65477167a5c11545a3193fc6c836ae730af21350/app/templates/grunt/config/util/injector.js#L95), I hope you can tell me how to include automatically all my Bower-installed Sass extensions.

## Test

Simply `cd` to `test/` folder and run a regular Sass compiler:

```bash
sass test.sass test.css
```

Or do it with your favorite Sass tool like [CodeKit](http://incident57.com/codekit/), [Prepros](http://alphapixels.com/prepros/), [grunt-contrib-sass](https://github.com/gruntjs/grunt-contrib-sass), [gulp-ruby-sass](https://github.com/sindresorhus/gulp-ruby-sass) or anything else.

## Reference

- There is an issue [#122](https://github.com/postcss/autoprefixer/issues/122) on Autoprefixer that depends on Can I Use database update. When it will be updated, this repository will not needed anymore for those who using Autoprefixer. But we need to wait. I hate waiting.

## Thanks

- Thank you [David Lindkvist](https://gist.github.com/ffdead) for [SASS resolution media query mixin](https://gist.github.com/ffdead/4215169), this repository based on his gist.
- Thank you [Andrey Sitnik](https://github.com/ai) for [Autoprefixer](https://github.com/postcss/autoprefixer), my work are inspired by first early releases of your great tool that breaks the chains of prefixes.
