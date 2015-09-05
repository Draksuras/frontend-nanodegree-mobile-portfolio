## Website Performance Optimization portfolio project

## Running the application:

    -Open index.html in Chrome (or other internet browser)

## Outline of Optimizations

# For index.html:

In index.html to reduce the critical rendering path length, number of critical resources, and critical resource size,
the following changes were made:

    -All CSS was inlined to reduce time spent sending/receiving external CSS files
    -JS scripts (both external and inline) were either placed at the bottom of the page or had the async attribute added to prevent
     render blocking
    -Web font loading (part of the JS) was placed at the bottom of the page to reduce the impact of the time spent retrieving the fonts
    -Images were optimized to reduce their file size
    -Finally, the HTML was minified to reduce file size

# For pizza.html (i.e. modifying main.js):

In main.js to increase the FPS to 60 the following changes were made:

    -The function 'determineDx' was removed as it performed many calculations that effectively did very little.
    -The changePizzaSizes function contained a for loop which was causing layout thrashing and wastefully collecting
     a list of pizza objects in each loop iteration, in addition to calling the 'determineDx' function
        -First change was placing the collection of pizza objects in a variable outside of the for loop and
         the second change was replacing querySelectorAll with the faster getElementsByClassName function to obtain the
         pizza objects list
        -Finally, the width resizing of the pizza's was changed to a percentage of the width

    -The for loop which creates the animated background pizzas was producing (an unnecessary) 200 when only a small number were
     shown on screen. The number was reduced to 50.

    -The updatePositions function was calculating multiple items that were 1) Re-calculated every time the mouse
     was scrolled and 2) Inside a for loop iterating over the pizza objects. This greatly reduced the FPS and
     the following fixes were made:

     -First, querySelectorAll was again replaced with getElementsByClassName
     -The 'items' variable collecting all the animated pizzas was placed in the global space so that it was not
      needlessly called every time the mouse was scrolled
     -The call for document.body.scrollTop was moved to its own function, getScrollY to reduce needless calls
      and calculations
     -The 'phase' variable which used document.body.scrollTop was moved to a separate global function,
      calculatePhases, again to reduce needless calls/caculations. The phases were then precalculated and
      called by updatePositions.
    
    -Finally, added CSS properties will-change: transform and transform: translateZ(0) to the .mover class
     so that only the animated pizzas would be painted (rather than the entire document) whenever they 
     moved. It also offloads the rendering to the GPU.
