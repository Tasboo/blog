---
title: "Messing with Decorators"
date: "2019-12-05T04:00:00+01:00"
draft: false
---

Woo, its been a few months since I've done anything with this. I started getting curious about Rails development again and decided to poke around at what other Rails developers are using in their apps. One item I came across was using decorators to manage presentation logic. 

<h3>Draper</h3>
The library I found was Draper. Draper allows you to add decorators to new and existing models pretty easily.

<h3>Installing Draper</h3>
The concept of install Ruby gems is still pretty new to me. So it was a bit of a head scratcher on what exactly I needed to do. The installation instructions say to add Draper to your Gemfile. At this point as a complete Ruby and Rails noob, I have no clue what that means. I finally figured it out though.

In directory of the rails app, there is a file called Gemfile. Within Gemfile, you can add different gems for your Ruby app to use. In my case, there were already a good amount of gems that were added automatically to the Gemfile from when I first setup the rails app. 

<img src="/images/Gemfile.png" />

<h3>Adding Gems</h3>
I hadn't had to open the Gemfile before, so doing this part was kind of new. After some minor headbanging, I figured out the exact steps I needed to do:

1. In my rails app's directory, open the Gemfile file.
2. In the Gemfile, add this line:
```
  gem 'draper'
```
3. In your terminal window, navigate to my rail's app directory and run the following command:
```
  bundle install
```

That's it. That simple. All of this confusion is because I am a noob. Up until this point, I have not had to do anything with the Gemfile before. Now that I know it is there and how to install gems, I feel a lot more comfortable with that whole process.

<h3>Using Draper</h3>
The first thing I did was run `rails generate draper:install`.

This added the file myrailsapp/app/decorators/application_decorator.rb
This seems to be useful for adding methods that would be helpful for all decorators across all your models. I could see this being used for different conversions

The next thing I did was to add a decorator to my existing animal model by running `rails generate decorator Animal`.

This added the file myrailsapp/app/decorators/animal_decorator.rb

In animal_decorator.rb I added the following decorator method which will append the text " - Decorated!" to the animal's name:

```
class AnimalDecorator < Draper::Decorator
  delegate_all

  # Define presentation-specific methods here. Helpers are accessed through
  # `helpers` (aka `h`). You can override attributes, for example:
  #
  #   def created_at
  #     helpers.content_tag :span, class: 'time' do
  #       object.created_at.strftime("%a %m/%d/%y")
  #     end
  #   end
  def decorated_name
    "#{object.name} - Decorated!"
  end
end
```

I then updated the show method in AnimalsController to call the .decorate method on the model:
```
def show
  @animal = Animal.find(params[:id]).decorate
end
```

Lastly, in views/animals/show.html.erb, I updated the view to use @animal.decorated_name instead of @animal.name:

```
<p>
  <span>Name:</span>
  <%= @animal.decorated_name %>
</p>
```

And that was it. Here is the result:
<img src="/images/DecoratorInUse.png" />

<h3>Conclusions</h3>
Not too shabby. While I really don't have a great use-case for decorators in my app at this point, I could see this being really useful for organizing the view-model logic in an app, especially in cases where you would want a particular field that is present in many view models to have the same output across all of your views. There are other ways to do it, I'm sure, but this seems like a nice clean way of doing it.


