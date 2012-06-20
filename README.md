# This is a fork (of a fork) of Bootstrap Typeahead that adds minimal but powerful extensions.
Original Bootstrap code :: http://twitter.github.com/bootstrap/assets/js/bootstrap-typeahead.js

Original Fork code :: https://gist.github.com/1866577

This Fork (you are currently viewing) :: https://gist.github.com/2960885

##Original Fork adds:##

1. Ajax source

2. Source as an object (with property parameter to determine which value in object to use)

3. Onselect callback

4. Up/Down to select options

##My Fork Adds:##

1. minLength parameter- ala jquery ui autocomplete. Requires textbox to have a set length before calling source.

2. By default no options are selected. If you tab while no option is selected it will leave your textbox value as is.

3. Up/Down - when it reaches the start or finish the next press will deselect all options, then next press will start scroll over options again. This is needed to allow tab off of an input.

4. Left/Right no longer refresh datasource. There will probably be other keys we want to ignore.

### For example, process typeahead list asynchronously and return objects

```coffeescript
  # This example does an AJAX lookup and is in CoffeeScript
  $('.typeahead').typeahead(
    # source can be a function
    source: (typeahead, query) ->
      # this function receives the typeahead object and the query string
      $.ajax(
        url: "/lookup/?q="+query
        # i'm binding the function here using CoffeeScript syntactic sugar,
        # you can use for example Underscore's bind function instead.
        success: (data) =>
          # data must be a list of either strings or objects
          # data = [{'name': 'Joe', }, {'name': 'Henry'}, ...]
          typeahead.process(data)
      )
    # if we return objects to typeahead.process we must specify the property
    # that typeahead uses to look up the display value
    property: "name"
  )
```

### For example, process typeahead list synchronously and fire a callback on selection

```javascript
  // This example is in Javascript, collects html in some li's and returns it
  $('.typeahead').typeahead({
    source: function (typeahead, query) {
      var return_list = []
      $("li").each(function(i,v){
        return_list.push($(v).html())
      })
      // here I'm just returning a list of strings
      return return_list
    },
    // typeahead calls this function when a object is selected, and
    // passes an object or string depending on what you processed, in this case a string
    onselect: function (obj) {
      alert('Selected '+obj)
    }

  })
```

### and a very simple example, showing you can pass list of objects as source, and get that object via onselect
```javascript
  $('.typeahead').typeahead({
    // note that "value" is the default setting for the property option
    source: [{value: 'Charlie'}, {value: 'Gudbergur'}, ...],
    onselect: function(obj) { console.log(obj) }
  })
```

Note that onselect works without source as a function and vice versa. Events may be a cleaner solution to passing callbacks and using bind all over the place, but I tried to strike a balance between modifying the core source too much and adding functionality, so until further improvements on the original Typeahead source I think these additions are very helpful.