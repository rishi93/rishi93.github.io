---
layout: posts
title:  "Welcome to Jekyll!"
date:   2023-06-13 13:55:25 +0200
categories: jekyll update
---
## Getting started with Jekyll
First install `bundler` and `jekyll`.
```bash
gem install jekyll bundler
```

Use the `jekyll` command line that you just installed to create a new jekyll site.
```bash
jekyll new myblog
```

Change into the newly created jekyll site.
```bash
cd myblog
```

Start the development server.
```bash
bundle exec jekyll serve --livereload
```

## Ruby snippets
```ruby
class Person
  def init(name, age)
    @name = name
    @age = age
  end

  def greet
    puts "Hi! My name is #{@name}"
  end
end
```
