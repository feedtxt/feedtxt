# feedtxt gem - reads Feed.TXT - feeds in text (unicode) - publish & share posts, articles, podcasts, 'n' more


* home  :: [github.com/feedtxt/feedtxt](https://github.com/feedtxt/feedtxt)
* bugs  :: [github.com/feedtxt/feedtxt/issues](https://github.com/feedtxt/feedtxt/issues)
* gem   :: [rubygems.org/gems/feedtxt](https://rubygems.org/gems/feedtxt)
* rdoc  :: [rubydoc.info/gems/feedtxt](http://rubydoc.info/gems/feedtxt)
* forum :: [groups.google.com/group/wwwmake](http://groups.google.com/group/wwwmake)


## Usage

Use `Feedtxt::Parser.parse` to read / parse feeds in text (using the Feed.TXT)
format.
The parse method will return an array:

```
[ feed_metadata,
  [
    [ item_metadata, item_content ],
    [ item_metadata, item_content ],
    ...
  ]
]
```

- The 1st element is the feed metadata hash.
- The 2nd element is the items array.
  - The 1st element in an item array is the item metadata hash.
  - The 2nd element in an item array is the item content.

Easier to see it in action. Let's read in:

``` ruby
require 'feedtxt'

text =<<TXT
|>>>
title:          "My Example Feed"
home_page_url:  "https://example.org/"
feed_url:       "https://example.org/feed.txt"
</>
id:  "2"
url: "https://example.org/second-item"
---
This is a second item.
</>
id:  "1"
url: "https://example.org/initial-post"
---
Hello, world!
<<<|
TXT

feed = Feedtxt::Parser.parse( text )
pp feed
```

resulting in:

``` ruby
[
  {"title"        =>"My Example Feed",
   "home_page_url"=>"https://example.org/",
   "feed_url"     =>"https://example.org/feed.txt"
  },
  [[
     {"id" =>"2",
      "url"=>"https://example.org/second-item"
     },
     "This is a second item."
   ],
   [
     {"id"=>"1",
      "url"=>"https://example.org/initial-post"
     },
     "Hello, world!"
  ]]
]
```

and use like:

``` ruby

feed_metadata = feed[0]
feed_items    = feed[1]

feed_metadata[ 'title' ]
# => "My Example Feed"
feed_metadata[ 'feed_url' ]
# => "https://example.org/feed.txt"

item          = feed_items[0]   # or feed[1][0]
item_metadata = item[0]         # or feed[1][0][0]
item_content  = item[1]         # or feed[1][0][1]

item_metadata[ 'id' ]
# => "2"
item_metadata[ 'url' ]
# => "https://example.org/second-item"
item_content
# => "This is a second item."

item          = feed_items[1]    # or feed[1][1]
item_metadata = item[0]          # or feed[1][1][0]
item_content  = item[1]          # or feed[1][1][1]

item_metadata[ 'id' ]
# => "1"
item_metadata[ 'url' ]
# => "https://example.org/initial-post"
item_content
# => "Hello, world!"
...
```

Another example. Let's try a podcast:


``` ruby
text =<<TXT
|>>>
comment: "This is a podcast feed. You can add..."
title:   "The Record"
home_page_url: "http://therecord.co/"
feed_url:      "http://therecord.co/feed.txt"
</>
id:        "http://therecord.co/chris-parrish"
title:     "Special #1 - Chris Parrish"
url:       "http://therecord.co/chris-parrish"
summary:   "Brent interviews Chris Parrish, co-host of The Record and one-half of Aged & Distilled."
published: 2014-05-09T14:04:00-07:00
attachments:
- url:           "http://therecord.co/downloads/The-Record-sp1e1-ChrisParrish.m4a"
  mime_type:     "audio/x-m4a"
  size_in_bytes: 89970236
  duration_in_seconds: 6629
---
Chris has worked at [Adobe][1] and as a founder of Rogue Sheep, which won an Apple Design Award for Postage.
Chris's new company is Aged & Distilled with Guy English - which shipped [Napkin](2),
a Mac app for visual collaboration. Chris is also the co-host of The Record.
He lives on [Bainbridge Island][3], a quick ferry ride from Seattle.

[1]: http://adobe.com/
[2]: http://aged-and-distilled.com/napkin/
[3]: http://www.ci.bainbridge-isl.wa.us/
<<<|  
TXT

feed = Feedtxt::Parser.parse( text )
pp feed
```

resulting in:

``` ruby
[{"comment"=>"This is a podcast feed. You can add...",
  "title"=>"The Record",
  "home_page_url"=>"http://therecord.co/",
  "feed_url"=>"http://therecord.co/feed.txt"
 },
 [
   [{"id"=>"http://therecord.co/chris-parrish",
     "title"=>"Special #1 - Chris Parrish",
     "url"=>"http://therecord.co/chris-parrish",
     "summary"=>"Brent interviews Chris Parrish, co-host of The Record and...",
     "published"=>2014-05-09 23:04:00 +0200,
     "attachments"=>
      [{"url"=>"http://therecord.co/downloads/The-Record-sp1e1-ChrisParrish.m4a",
        "mime_type"=>"audio/x-m4a",
        "size_in_bytes"=>89970236,
        "duration_in_seconds"=>6629}]
     },
     "Chris has worked at [Adobe][1] and as a founder of Rogue Sheep..."
   ]
 ]
]
```

and use like:

``` ruby
feed_metadata = feed[0]
feed_items    = feed[1]

feed_metadata[ 'title' ]
# => "The Record"
feed_metadata[ 'feed_url' ]
# => "http://therecord.co/feed.txt"

item          = feed_items[0]  # or feed[1][0]
item_metadata = item[0]        # or feed[1][0][0]
item_content  = item[1]        # or feed[1][0][1]

item_metadata[ 'title' ]
# => "Special #1 - Chris Parrish"
item_metadata[ 'url' ]
# => "http://therecord.co/chris-parrish
item_content
# => "Chris has worked at [Adobe][1] and as a founder of Rogue Sheep..."
...
```


## License

![](https://publicdomainworks.github.io/buttons/zero88x31.png)

The `feedtxt` scripts are dedicated to the public domain.
Use it as you please with no restrictions whatsoever.

## Questions? Comments?

Send them along to the [wwwmake Forum/Mailing List](http://groups.google.com/group/wwwmake).
Thanks!
