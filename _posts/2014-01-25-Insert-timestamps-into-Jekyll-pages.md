---
layout: post
title: Insert timestamps into Jekyll pages
category: digital
tags: tech jekyll
---

Though my [Jekyll setup](/2014/01/19/maplesteve-now-powered-by-jekyll) runs perfectly smooth, I wanted to have some kind of control, if (all) pages are re-generated when I make changes to the CSS or other minor things that aren't immediately visible.

So I wanted to have a timestamp of the page generation.
To have re-usable code, I was looking for a custom Liquid tag for that purpose and found a gist by blakesmith: [render_time.rb](https://gist.github.com/blakesmith/449509).

Since I wanted the timestamp only as a comment in the HTML pages, I made a small change. Besides that, it's the same - so thanks a lot [blakesmith](https://github.com/blakesmith)!

Here's render_time.rb:

```
module Jekyll
  class RenderTimeTag < Liquid::Tag
 
    def initialize(tag_name, text, tokens)
      super
      @text = text
    end
 
    def render(context)
      "<!-- #{@text} #{Time.now} -->"
    end
  end
end
 
Liquid::Template.register_tag('render_time', Jekyll::RenderTimeTag)
```

Just drop it in your ```_plugins``` folder and add the line (enclosed in Liquid open and close tags; which I can't do here, because the Liquid tag would then be executed... :-/)

```render_time Page generated at:```

to e.g. your ```default.html```.

