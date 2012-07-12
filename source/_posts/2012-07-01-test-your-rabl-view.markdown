---
layout: post
title: "Test your Rabl's view"
date: 2012-07-03 09:40
comments: true
categories:
  - test
  - rspec
  - rabl
---
Since several month, I use only [Rabl](https://github.com/nesquena/rabl)
to do all my API. To me this gems is really awesome. Before using it, I
try doing all of my API with the `#to_json` method. But it was a
nightmare. If you already try to do that, you understand what I mean.
The most bad case is when you want represent your object in different
way depending of where is represent.

# Rabl simply clean

To manage your JSON and XML view, the most common gem is `JBuilder` and
`XmlBuilder`, both are created by DHH. To me it's not a good DSL and can
be a little too verbose. Rabl has a clean a simplest DSL and you just
need one view to generate XML and JSON. You can even generate some other
output with Rabl.

A little example of Rabl's view :

```ruby
object @post
attribute :content
child(:author) do
  attribute :firstname, :lastname
end
```

# Test your Rabl's view

To test the output of this view, you have multiple choice. You can
generate this view in an integration test and test this output. You can
do it in your controller test. Or the best, test only your output on
your view test. With this last choice, you can define what you really
put on your view and test only your rabl representation. In this case
you can have a flexible and fast test.

Since Rabl 0.6.0, a new method help to generate your view directly. It's
with this method you can simply test our view.

An example of how we can test our view (
`spec/views/posts/show_rabl_spec.rb`) :

```ruby
require 'spec_helper'

describe "blogs/show.rabl" do

    let(:auhor) { Author.new(:firstname => 'Cyril', :lastname => 'Mougel') }
    let(:blog) { Blog.new(:content => 'hello', :author => author) }
    let(:valid_json) {
      {
        :content => blog.content,
        :author => {
          :firstname => author.firstname,
          :lastname => author.lastname
        }
      }.to_json
    }

    let(:render) {
      Rabl.render(
        blog,
        'blog/show',
        :view_path => 'app/views'
      )
    }

    it 'should render valid_json' do
      render.should eql(valid_json)
    end

end
```

We have a really simple test unit with a complet JSON validation. This
test is fast because no need doing some database request and no
controller stuff needed.

But, I found a little limitation in this method. You can't pass some
locals variables. So you can only test this render with value pass on
the first params. If you have some other object you want use, you can't.
I start a [Pull Request](https://github.com/nesquena/rabl/pull/261) to
pass this variables in a `:locals` options. You can
[follow it on github](https://github.com/nesquena/rabl/pull/261)

[Traduction Française](http://blog.shingara.fr/tester-ses-vues-rabl.html)
