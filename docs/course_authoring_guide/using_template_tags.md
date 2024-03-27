# Using Template Tags

KinesinLMS supports a small number of Django template tags in the HTML content portions of the course.
These tags are used to insert dynamic content into the course content.

The "Enable template tags" option must be enabled in the block settings for the tags to be processed.

Currently supported tags:

## module_link
Link to a module by its content index.

###Example

This HTML content entered in Composer:
```
...more about this in {{ module_link 3 }}....
```
...will generate HTML like:
```html
...more about this in <a href="/course/1/module/3/">Module 3</a>....
```


