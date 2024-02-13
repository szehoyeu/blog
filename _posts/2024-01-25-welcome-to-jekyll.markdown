---
layout: post
title:  "Welcome to Jekyll!"
date:   2024-01-25 22:27:42 +0000
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


Overriding theme defaultsPermalink
---
[Link: Theme](https://jekyllrb.com/docs/themes/)
Jekyll themes set default data, layouts, includes, and stylesheets. However, you can override any of the theme defaults with your own site content.

To replace layouts or includes in your theme, make a copy in your _layouts or _includes directory of the specific file you wish to modify, or create the file from scratch giving it the same name as the file you wish to override.

For example, if your selected theme has a page layout, you can override the theme’s layout by creating your own page layout in the _layouts directory (that is, _layouts/page.html).

To locate a theme’s files on your computer:

Run bundle info --path followed by the name of the theme’s gem, e.g., bundle info --path minima for Jekyll’s default theme.

This returns the location of the gem-based theme files. For example, the Minima theme’s files might be located in /usr/local/lib/ruby/gems/2.6.0/gems/minima-2.5.1 on macOS.