# HitCounter

[![Travis CI](https://secure.travis-ci.org/ivanoblomov/hit_counter.png)](http://travis-ci.org/ivanoblomov/hit_counter)
[![Code Climate](https://codeclimate.com/github/ivanoblomov/hit_counter.png)](https://codeclimate.com/github/ivanoblomov/hit_counter)
[![Coveralls](https://coveralls.io/repos/ivanoblomov/hit_counter/badge.svg?branch=master&service=github)](https://coveralls.io/github/ivanoblomov/hit_counter?branch=master)
[![Dependency Status](https://gemnasium.com/ivanoblomov/hit_counter.png)](https://gemnasium.com/ivanoblomov/hit_counter)
[![Gem Version](https://badge.fury.io/rb/hit_counter.svg)](http://badge.fury.io/rb/hit_counter)

Ruby version of that old 90s chestnut, `<BLINK>`the web-site hit counter`</BLINK>`.

## Installation

1. Install the gem and supporting files.

`Gemfile`
```ruby
  gem 'hit_counter'
```

Run:

```bash
  bundle
  rake hit_counter:install
```

2. Add a controller action to your app.

`application_controller.rb`
```ruby
  def hit_counter
    return if params[:url].blank?

    # find or create a hit counter for this URL
    hc = HitCounter.get params[:url]

    # increase the tally by one
    hc.increment

    # get the image as a blob and stream it to the browser
    send_data hc.image(params[:style]).to_blob, :disposition => 'inline', :filename => "#{hc.hits}.png", :type => :png
 end
```

`routes.rb`
```ruby
 get 'hit-counter' => 'application#hit_counter' # technically should be POST/PUT, but GET makes integration simpler
````

3. Add the hit-counter image tag to your site's HTML:

```html
  <img alt="Hit Counter" border="0" src="/hit-counter?url=http://cnn.com&style=1" />
````

## Customizing the Hit-Counter Image

### Use an Existing Style

You can use one of the three included styles by specifying a different `style` param in the HTML:

```html
  <img alt="Hit Counter" border="0" src="/hit-counter?url=cnn.com&style=1" />
```

The included styles are:

![image of 1 in odometer style](http://github.com/ivanoblomov/hit_counter/raw/master/public/images/digits/odometer/1.gif)
Odometer

![image of 2 in scout style](http://github.com/ivanoblomov/hit_counter/raw/master/public/images/digits/scout/2.gif)
Scout

![image of 3 in celtic style](http://github.com/ivanoblomov/hit_counter/raw/master/public/images/digits/celtic/3.gif)
Celtic

### Create Your Own Style

1. To add your own style, create a GIF for each of the digits, `0.gif` through `9.gif`. Save the images in a folder named after your new style in `public/images/digits`.

2. In your controller, declare a new `STYLES` constant and add the folder name to it:

```ruby
  HitCounter::STYLES = ['odometer', 'scout', 'celtic', '(folder name)']
```

3. In your HTML, specify the new style with a `style` param equal to its index in the array plus one (unlike arrays, the `style+ param's index is one-based):

```html
  <img alt="Hit Counter" border="0" src="/hit-counter?url=cnn.com&style=4" />
```

## Contributing to hit_counter

* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it
* Fork the project
* Start a feature/bugfix branch
* Commit and push until you are happy with your contribution
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Copyright

Copyright (c) 2011 Roderick Monje. See LICENSE.txt for further details.