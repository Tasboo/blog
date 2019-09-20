---
title: "New controller action"
date: "2019-09-20T07:10:00+01:00"
draft: false
---

Now that I [have created my first controller](/blog/FirstControllerInRails), I wanted to explore adding additional actions/views to the controller. In all, it wasn't too bad.

To create the new action, I manually defined a new action in the home controller. To create the corresponding view, I manually created the view file in the views/home directory. 

For this new action, I decided I wanted to explore using parameters. The goal was to have a new action called 'animal' which could accept a parameter of 'animal'. The animal parameter would allow the user to specify what animal they wanted to see. To keep it simple, I decided to only have 2 choices available: cat or dog.

First thing to do was to grab the images for the cat and dog. I went out to <a href="https://unsplash.com" target="_blank">unsplash</a> which offers a great selection of images to use for free. After downloading the images I wanted, I put them in the assets directory within the app:

![image files](/images/assetsimages.png)

Note that I did not add the two files with the :Zone.Identifier names. Rails created those files automatically when I ran the site. I only added the cat.jpg and dog.jpg files.

Moving on to the code, I decided the simplest way to get this functionality working was to specify the animal selection using a query parameter. So to view a cat you would navigate to '/home/animal?animal=cat'. To accomplish this, I wired up my home controller like so:

```
class HomeController < ApplicationController
  def index
  end
  def animal
    @animal = params[:animal]
    if @animal == "cat"
      @animalPicture = "cat.jpg"
      @credit = "<a style=\"background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px\" href=\"https://unsplash.com/@edgaredgar?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge\" target=\"_blank\" rel=\"noopener noreferrer\" title=\"Download free do whatever you want high-resolution photos from Edgar Edgar\"><span style=\"display:inline-block;padding:2px 3px\"><svg xmlns=\"http://www.w3.org/2000/svg\" style=\"height:12px;width:auto;position:relative;vertical-align:middle;top:-2px;fill:white\" viewBox=\"0 0 32 32\"><title>unsplash-logo</title><path d=\"M10 9V0h12v9H10zm12 5h10v18H0V14h10v9h12v-9z\"></path></svg></span><span style=\"display:inline-block;padding:2px 3px\">Edgar Edgar</span></a>"  
    elsif @animal == "dog"
      @animalPicture = "dog.jpg"
      @credit = "<a style=\"background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px\" href=\"https://unsplash.com/@jamie452?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge\" target=\"_blank\" rel=\"noopener noreferrer\" title=\"Download free do whatever you want high-resolution photos from Jamie Street\"><span style=\"display:inline-block;padding:2px 3px\"><svg xmlns=\"http://www.w3.org/2000/svg\" style=\"height:12px;width:auto;position:relative;vertical-align:middle;top:-2px;fill:white\" viewBox=\"0 0 32 32\"><title>unsplash-logo</title><path d=\"M10 9V0h12v9H10zm12 5h10v18H0V14h10v9h12v-9z\"></path></svg></span><span style=\"display:inline-block;padding:2px 3px\">Jamie Street</span></a>"
    else
      @animalPicture = ""
      @credit = ""
    end
  end
end
```

Lets walk through what I did:

- 'def animal' defined a new action in my controller
- '@animal = params[:animal]' assigned a variable named animal to the value of a parameter named animal.
- the if/elsif statement determines what content to show on the page.
  - just a note, for someone not used to Ruby development, using 'elsif' instead of 'elseif' (note missing 'e' in the first one) through me for a loop. It took me a few minutes to figure out that syntax 😝.
- In the if/elsif blocks, I assign 2 variables. First variable is the image file name I want to display. The second variable is an image credit to the unsplash site (not required for using these photos, but it seems like the right thing to do).

Pretty simple controller logic. 

Next thing to do was to create the view itself. This required creating a view called animals.html.erb in the views/home directory.

![animal view](/images/animalview.png)

Within that view, I added the following code to display the variables from the new animal action in the home controller:

```
<h2><%= @animal %></h2>
<%=image_tag(@animalPicture, width: '500')%>
<div>
<%= sanitize @credit %>
</div>
```

Walking through the code:
- I output the selected animal using the @animal variable in the code '<%= @animal %>'
- I create an image tag using the image_tag method. This allows you to display images from the assets/images directory.
- I output the @credit variable using the 'sanitize' helper. This allows me to output the @credit variable as html while also sanitizing the value to be more secure.

Next I create links for selecting cat or dog from the index.html.erb view that I had created in my previous post.

```
<h2>Home</h2>
<h3>Pick an animal!</h3>
<div>
    <div>
        <a href="/home/animal?animal=cat">cat</a>
    </div>
    <div>
        <a href="/home/animal?animal=dog">dog</a>
    </div>
</div>
```

Notice how I have specified the animal query parameter in the URL to indicate what animal I want to view.

Last thing I needed to do was to setup the route to the new action. I opened up the config/routes.rb file to define the route for the animal action:

```
Rails.application.routes.draw do
  get 'home/index'
  get 'home/animal'
  root 'home#index'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

To test it out I run the site and click on the dog and cat links to make sure they work correctly.

![selecting an animal](/images/animalhome1.png)
![animal action view result](/images/animalactionviewresult.png)

Success! As you can see, the h2 tag has the specified animal name, the correct image is displayed, and the credit is output. Looking at the credit, it looks like the santize helper modified the html a bit. No big deal, but interesting to see what it did. 

There is one more thing I wanted to try for this. I wanted that URL to be cleaned up a little better. The URL '/home/animal?animal=cat' is rather clunky looking to me. Lets go for '/home/animal/cat' instead. This is possible without having to modify my controller at all. I just have to update the config/routes.rb file so that the /cat gets passed in as a parameter. Here are my updated routes:

```
Rails.application.routes.draw do
  get 'home/index'
  get 'home/animal'
  get 'home/animal/:animal', to: 'home#animal'
  root 'home#index'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

The new entry is a little more complicated than the previous two. The first part of the new route (get 'home/animal/:animal') adds an entry for a new route with a parameter called animal at the tail end of it. The second part (, to: 'home#animal') tells rails which controller and action to call when that URL is accessed. Basically, it is telling rails that if the user specifies /home/animal/cat, send the user to the animal action in the home controller and pass in cat as a parameter.

I update index.html.erb to have two options of selecting animals, one with query parameters, and one with route parameters:

```
<h2>Home</h2>
<h3>Pick an animal using query string parameters!</h3>
<div>
    <div>
        <a href="/home/animal?animal=cat">cat</a>
    </div>
    <div>
        <a href="/home/animal?animal=dog">dog</a>
    </div>
</div>
<h3>Pick an animal using route parameters!</h3>
<div>
    <div>
        <a href="/home/animal/cat">cat</a>
    </div>
    <div>
        <a href="/home/animal/dog">dog</a>
    </div>
</div>
```

And here is the result:

![selecting an animal with route parameters](/images/animalhome2.png)
![animal action view result 2](/images/animalactionviewresult2.png)

Success! Looking at the URL, you can see it now looks like this after I click the dog link: '/home/animal/dog'. The /dog part of the URL is now the parameter. What I like about this solution is that I didn't need to modify my controller to get this functionality to work. The home controller has the same exact code as before when we were accessing the animal using the query string parameter. In fact, the query string links on the home page still work as well because the routing still supports the original URL route that we first added. So that means I can access the animal using '/home/animal/dog' or '/home/animal?animal=dog'. 😺🐶

That's it for this post. There are a few different topics I covered here, but now that I am through this part, I know that I can start plugging in different pages and functionality into the site.



