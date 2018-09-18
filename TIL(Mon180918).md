# Sun Sep 10 2018

## TIL(Today I Learned)
---

### jQuery

- Adding and Removing Elements

  While it is possible to insert elements into the DOM using the *html* method, this is suitable only for creating child elements of a particular element. Therefore, jQuery provides a number of methods for manipulating any part of the DOM.

  These methods are *append*, *prepend*, *after*,*before*, *remove*, and *empty*.

  The *prepend* method has been used to insert the string Link: before the inner text or HTML of all `<a>` elements, like this:

  ```
  $('a').prepend('Link:')
  ```

  Then an attribute selector is used to slect all elements that have an *href* attribute starting with *http*. The string *http* denotes links that are not relative (and therefore are absolute), in which case an external link icon is appended to the end of the inner text or HTML of all matching elements, like this:

  ```
  $("[href^='http']").append(" <img src='link.png'>")
  ```

  ※ The ^=operator is how only the start of the string is matched. If just the = operator were used, only entire strings that matched woudl be selected. 

  Next, using chianed methods, the *before* and *after* methods are employed to place sibling elements eigther before or after one. In this instance, I have chosen to place an `<hr>` element both before and after `<code>` elements, like this:

  ```
  $('code').before('<hr>').after('<hr>')
  ```

  Then I added a little user-interaction with a couple of buttons. When clicked, using the *remove* method, the first button removes the `<img>` element containing the ball, like this:

  ```
  $('#a').click(function() { $('#ball').remove() } )	
  ```

  Finally, the empty method is applied to the `<blockquote>` element when the second button is clicked, which simply empties out the element's contents, but leaves the element in the DOM, like this:

  ```
  $('#b').click(function() { $('#quote').empty() } )
  ```

- Dynamically Applying Classes

  Sometimes it can be convenient to change the class an element employs, or maybe just add a class to an element or remove it from one. For example, suppose tou have a class called *read* that you use to style blog posts that have been read. Using the *addClass* method, it's a simple matter to add a class to that post, like this:

  ```
  $('#post23').addClass('read')			
  ```

  you can add more than on class at a time by separating them with spaces, like this:

  ```
  $('#post23').addClass('read liked')
  ```

  But what if a reader chooses to mark a post as unread again, perhaps to be reminded to read it again later? In this case, all you need to do is use *removeClass*, lik ethis:

  ```
  $('#post23').removeClass('read')
  ```

  All other classes that the post uses remain unaffected when you do this:



  Where you are supporting the ability of a class to be continuosly added or removed, you might, however, find it simpler to use the *toggleClass* method, like this:

  ```
  $('#post23').toggleClass('read')
  ```

  Now, if the post doesn't use the class, it is added; otherwise, it is removed.

- $(this) selector

  $(this) selector refers or stands for the current selected object in a loop or event. 

  For example, if you have an unordered list `<ul>` with five list items `<li>` inside and you run a click event for each list item, when you click on a single item, jQuery $(this) selector will refer exactly to that clicked item. 



- practice ex:

  ```
  var tab = $('.tab');
  
  tab.attr('tabindex', '0');
  
  tab.on('click focusin', function () {
      $(this).parent().siblings().removeClass('tab-act');
      $(this).parent().addClass('tab-act');
  });
  ```
