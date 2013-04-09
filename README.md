
# MarkupSimpleNavigation 1.1.8

## Basic usage

Once installed you can load the module in your template code on demand using $modules variable:

```php
$menu = $modules->get("MarkupSimpleNavigation");
```

Simplest is to call render method with no options. It will return the markup string with a
nested UL markup tree on all levels expanded from the root page "/". Containing "parent"
and "current" class attributes on anchors by default. It doesn't print out directly so
you need to use echo to output the markup. This also allows to further manipulate the
returned string or pass it further from when using it in another function.

```php
$treeMenu = $modules->get("MarkupSimpleNavigation"); // load the module
echo $treeMenu->render(); // render default menu
```

**Arguments:**

The render() method accepts some configuration options.

render($options, $page, $rootPage)

- `$options`, is an array of options
- `$page`, is a page object for the current page
- `$rootPage`, is a page object for the root of the menu


## Default options


```php
$options = array(
    'parent_class' => 'parent',
    'current_class' => 'current',
    'has_children_class' => 'has_children',
    'levels' => true,
    'levels_prefix' => 'level-',
    'max_levels' => null,
    'firstlast' => false,
    'collapsed' => false,
    'show_root' => false,
    'selector' => '',
    'outer_tpl' => '<ul>||</ul>',
    'inner_tpl' => '<ul>||</ul>',
    'list_tpl' => '<li%s>||</li>',
    'list_field_class' => '',
    'item_tpl' => '<a href="{url}">{title}</a>',
    'item_current_tpl' => '<a href="{url}">{title}</a>',
    'xtemplates' => '',
    'xitem_tpl' => '<span>{title}</span>',
    'xitem_current_tpl' => '<span>{title}</span>'
)
echo $treeMenu->render($options);
```
### Same with comments

```php
$options = array(

    'parent_class' => 'parent',
    // overwrite class name for current parent levels

    'current_class' => 'current',
    // overwrite current class

    'has_children_class' => 'has_children',
    // overwrite class name for entries with children

    'levels' => true,
    // wether to output "level-1, level-2, ..." as css class in links

    'levels_prefix' => 'level-',
    // prefix string that will be used for level class

    'max_levels' => null,
    // set the max level rendered

    'firstlast' => false,
    // puts last,first class to link items

    'collapsed' => false,
    // if you want to auto-collapse the tree you set this to true

    'show_root' => false,
    // set this to true if you want to rootPage to get prepended to the menu

    'selector' => '',
    // define custom PW selector, you may sanitize values from user input

    'outer_tpl' => '<ul>||</ul>',
    // template string for the outer most wrapper. || will contain entries

    'inner_tpl' => '<ul>||</ul>',
    // template string for inner wrappers. || will contain entries

    'list_tpl' => '<li%s>||</li>',
    // template string for the items. || will contain entries, %s will replaced with class="..." string

    'list_field_class' => '', // string (default '') add custom classes to each list_tpl using tags like {field} i.e. {template} p_{id}

    'item_tpl' => '<a href="{url}">{title}</a>',
    // template string for the inner items. Use {anyfield} and {url}, i.e. {headline|title}, if field is of type image it will return url to image (first image if multiple)

    'item_current_tpl' => '<a href="{url}">{title}</a>',
    // template string for the active inner items.

    'xtemplates' => '',
    // specify one or more templates separated with a pipe | to use the xitem_tpl and xitem_current_tpl markup

    'xitem_tpl' => '<a href="{url}">{title}</a>',
    // same as 'item_tpl' but for xtemplates pages, can be used to define placholders

    'xitem_current_tpl' => '<a href="{url}">{title}</a>'
    // same as 'item_current_tpl' but for xtemplates pages
)
echo $treeMenu->render($options);
```

## Overwrite current page or root page

If you want to overwrite starting point for the root page to be another page you could do it like this:

```php
$rootPage = $pages->get("/en/"); // or any other page that has subpages
echo $treeMenu->render(null, null, $rootPage); // null at first is to not have to specify options, just use default
```

Or to even overwrite the current active page

```php
$currentPage = $pages->get(1242);
$rootPage = $pages->get("/en/");
echo $treeMenu->render(null, $currentPage, $rootPage);
```

## Default markup output example

```html
    <ul>
        <li><a href='/'>Home</a></li>
        <li class='parent has_children'><a href='/about/'>About22</a>
            <ul>
                <li><a href='/about/what/'>Child page example 1</a></li>
                <li><a href='/about/background/'>Child page example 2</a></li>
                <li class='parent has_children'><a href='/about/test/'>test</a>
                    <ul>
                        <li class='current'><a href='/about/test/test2/'>test2</a></li>
                    </ul>
                </li>
            </ul>
            </li>
        <li class='has_children'><a href='/templates/'>Templates</a>
            <ul>
                <li><a href='/templates/template2/'>template2</a></li>
            </ul>
        </li>
        <li><a href='/site-map/'>Site Map</a></li>
    </ul>
```

## alternative urls (new in 1.1.8)

You can now use page fields to set an alternative redirect url to another page. For example {pagefieldname|url} for a item href. Where `pagefieldname` is a page field setup to dereference as "single or false" which is used to get a url to another page.

Usage i.e:

```item_tpl' => "<a href='{redirect_page|url}'>{title}</a>```

## "list field class" option (new in 1.1.6)

You can optionally define custom classes using a string that can contain tags for field names of pages.
For example `'list_field_class' => '{template} p{id}'`, will add `' basic-page p1001'` to each `list_tpl` class string.

## "xtemplates" Markup templates (new in 1.1.5)

You can now define custom markup for the item tpl's for pages with template(s) specified using `xtemplates`.
You can specify multiple templates by using `'basic-page|news'`.
Items with this templates will then use the `xitem_tpl'` and `xitem_current_tpl` instead of the standard `item_tpl` and `item_current_tpl`.

## Markup templates (new in 1.1.0)

If you want to define your own markup for the output you can use the options template to overwrite default.

Classes placeholder `%s`
is used in either list or inner list item to define where the classes will be appended (current, parent, etc)

Fields placeholder `{anyfield}`
Can be used on item tpl to output fields you want. You can also use `{headline|title}` so if headline is
empty it will chose title. If it's an image field, it will return the url to image. To output url you
just use `{url}`.


## Installation

1. Put the module folder named as MarkupSimpleNavigation into your site/modules/ folder
2. Go to modules install page and click "check for new modules"
3. Install the module. The module appears under "Markup" section.

