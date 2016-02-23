# responsive-helper
Sass helper for managing responsive media queries on projects

## Install
You can install responsive-helper with bower using the following command
```shell
$ bower install responsive-helper
```

## Usages

### Setting responsive breakpoints
Firstable you have to set your responsive breakpoints, based on this model (which is !default values):

```scss
$breakpoints: (
  'xs': ('min': 0, 'max': 374px),
  'sm': ('min': 375px, 'max': 524px),
  'md': ('min': 525px, 'max': 959px),
  'lg': ('min': 960px, 'max': 1199px),
  'xl': ('min': 1200px, 'max': '∞')
) !default;
```

`xs`, `sm`, `md` and `lg` are what we will call the "breakpoints tags", they are all the key of an another map defining the min-width and max-width fork that is defining the breakpoint tag. Notice the value `∞` meaning that "lg" breakpoint do not have any max-width.

### Using mixin
This map will allow you to use the `media-query` mixin made to generate media queries based on your breakpoint definition. Example below :
```scss
.selector {
    @include media-query('sm', 'restrictive') {
        // your code
    }
}
```

There is to parameters to the mixin:
* The breakpoint tag refering to the `$breakpoints` map variable you defined before
* The responsive philosophy of the media query. It can be `mobile-first`, `desktop-first` or `restrictive`.
    * `mobile-first` will generate a query based on the min-width of the breakpoint tag
    * `desktop-first` will generate a query based on the max-width of the breakpoint tag
    * `restrictive` will generate a query based on the min-width and the max-width of the breakpoints tag

#### Example
```scss
.selector {
    @include media-query('sm', 'mobile-first') {
        position: relative;
    }
}
// Will compile into
@media (min-width: 768px) {
    .selector {
        position: relative;
    }
}

.selector {
    @include media-query('sm', 'desktop-first') {
        position: relative;
    }
}
// Will compile into
@media (max-width: 991ox) {
    .selector {
        position: relative;
    }
}

.selector {
    @include media-query('sm', 'restrictive') {
        position: relative;
    }
}
// Will compile into
@media (min-width: 768px) and (max-width: 991ox) {
    .selector {
        position: relative;
    }
}
```

### Helper Classes
Also based on the `$breakpoints` map, responsive-helper generates helper classes to hide or display on specific breakpoints an element.

#### Hidden
To hidden on a specific breakpoint follow the pattern :
```html
<div class="hidden--[breakpoint]"></div>
```

##### Example
```html
<!-- this div will not be displayed on viewport between 768px and 991px -->
<div class="hidden--sm"></div>
```


#### Visible
To make visible on a specific breakpoint follow the pattern
```html
<div class="visible--[breakpoint]-[display]"></div>
```

##### Example
```html
<!-- this div will not be displayed on viewport between 768px and 991px -->
<div class="visible--sm-block"></div>
```

As default, there is only three display type that are compiled (block, inline-block, inline) but you can add more by redefining the `$visible-displays` variable map.

You can avoid the generation of those two kind of helper classes by switching to false the `$hidden-helpers` variable. You can also change the format of the prefix ('hidden--' and 'visible--') by redefining the variables `$hidden-helpers-prefix` and `$visible-helpers-prefix`.
