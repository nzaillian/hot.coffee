Hot.coffee
----------

Hot.coffee is a CoffeeScript anti-framework for server-rendered web applications. 
If you like to server-render as much of your apps as you can get away with
and dress up pages with javascript for any UX you can't achieve in other ways,  
then this anti-framework is for you. If you're looking for
a comprehensive client-side MVC framework or similar, it definitely
is not (and you might even say is philosophically opposed).

Is this a joke? No, not really.

How does it work? 

Well, it does not do very much. 
Hot.coffee exposes a simple harness (via the "$.hot"
function) to acommodate the anti-pattern (or so some might call 
it) of using classes as nothing but a container for procedural logic
on your page. You expose a "preconditions" function on a class
(this might check for a body class or some other indication of context)
and if the preconditions are met, Hot.coffee creates an instance of the 
class. If you're using Hot.coffee with Turbolinks, you may wish to define
a "destroy" function on your class in which you cleanup any resources
that you'd like to cleanup before new instances of the class are created.
This function will be called automatically.

The old way
-----------
    class ProfilePage
      constructor: ->
        $("#friends").click @present_friends_modal

      present_friends_modal: (e) =>
        #  ...

    $(document).bind "ready page:change page:restore", ->
      if window._profile_page?
        window._profile_page.destroy()
        window._profile_page = null

      if $("body").hasClass("profile-page")
        window._profile_page = new ProfilePage


The Hot.coffee way
------------------

    class ProfilePage
      constructor: ->
        $("#friends").click @present_friends_modal

      present_friends_modal: (e) =>
        #  ...

      @preconditions: ->
        $("body").hasClass("profile-page")

    $.hot ProfilePage    