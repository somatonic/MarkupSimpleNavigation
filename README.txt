
INSTALL:
----------------------------------------------------------------------------------------------------------

Go to your module Install page and click "Check for new modules". It will appear under the section Markup. 
Click "install" button. Done. (Techically you don't even need to install it, after the load call it will 
be installed automaticly on first request. But it feels better). However it will not be "autoloaded" by 
Processwire unless you load it in one of your php templates.


BASIC USAGE:
----------------------------------------------------------------------------------------------------------

Once installed you can load it on demand in your templates by using $modules->get("MarkupSimpleNavigation") method.

Arguments:
render($options, $page, $rootPage)
$options, is an array of options
$page, is a page object for the current page
$rootPage, is a page object for the root of the menu

Simplest is to call with no options. It will render an nested UL markup tree on all levels expanded from the
root page "/". Containing "parent" and "current" class attributes on anchors by default.

$treeMenu = $modules->get("MarkupSimpleNavigation"); // load the module
echo $treeMenu->render(); // render default menu


ADVANCED CALL WITH ALL OPTIONS AVAILABLE:
----------------------------------------------------------------------------------------------------------

$options = array(
	'max_levels' => 2, // int (default null) set the max level rendered 
	'show_root' => true, // bool (default false) set this to true if you want to rootPage to get prepended to the menu (default
	'collapsed' => true, // bool (default false) if you want to auto-collapse the tree (in markup) you set this to true
	'css_current' => 'on', // string (default 'current') overwrite current class
	'css_parent' => 'is_parent', // string (default 'parent') overwrite class name for parent levels
	'css_levels_on' => true, // bool (default false) wether to output "level-1, level-2, ..." as css class in links
	'css_firstlast_on' => true // bool (default false) puts last,first class to link items
	'ul_id' => 'mainnav', // string (default '') set a id name for the topmost ul element
	'ul_class' => 'mainnav' // string (default '') set a class name for the topmost ul element
	'selector' => 'parent!=1008' // string (default '') define custom PW selector, you may sanitize values from user input
)
echo $treeMenu->render($options);


OVERWRITE CURRENT PAGE AND ROOTPAGE:
----------------------------------------------------------------------------------------------------------

If you want to overwrite starting point for the root page to be another page you could do it like this:

$rootPage = $pages->get("/en/"); // or any other page that has subpages
echo $treeMenu->render(null, null, $rootPage); // null at first is to not have to specify options, just use default

Or to even overwrite the current active page

$currentPage = $pages->get(1242); // or any other page object 
$rootPage = $pages->get("/en/"); // or any other page that has subpages
echo $treeMenu->render(null, $currentPage, $rootPage); // null at first is to not have to specify options, just use default


OUTPUT EXAMPLE:
----------------------------------------------------------------------------------------------------------

<ul>
	<li><a class='level-1 first' href='/'>Home</a></li>
	<li><a class='level-1 parent' href='/about/'>About22</a>
		<ul>
			<li><a class='level-2 first' href='/about/what/'>Child page example 1</a></li>
			<li><a class='level-2' href='/about/background/'>Child page example 2</a></li>
			<li><a class='level-2 last on' href='/about/test/'>test</a>
				<ul>
					<li><a class='level-3 first last' href='/about/test/test2/'>test2</a></li>
				</ul>
				</li>
		</ul>
		</li>
	<li><a class='level-1' href='/templates/'>Templates</a>
		<ul>
			<li><a class='level-2 first last' href='/templates/template2/'>template2</a></li>
		</ul>
		</li>
	<li><a class='level-1 last' href='/site-map/'>Site Map</a></li>
</ul>


JS MENUS, CSS, MARKUP TEMPLATES
----------------------------------------------------------------------------------------------------------

This module will only output the basic HTML markup. No styling or script are added.

Using this output together with the right css or js plugin, you can quickly produce any working 
multilevel navigation. This is up to you. There's also currently no way to define your own markup.
Although you could do a str_replace to change simple things, since it returns a string this is possible.
Or use js to modify, add prefixes or wrap elements.

