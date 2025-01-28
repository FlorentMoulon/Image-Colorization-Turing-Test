Create a webpage using HTML, CSS and JS that allow researcher to pass turing test on image to people.


# Ressource :
- images :
    - images ar store in the folder `images/original` and `images/recolorized`

# Pages :
- home page :
    - title : Image colorization turing test
    - english explaination text : You will be presented N images of 100*100 pixel, some of them are original picture, some are grayscale images colorized using on AI program. You will have 3s to look at an image and then you'll have to say if it was original or recolorized. At end, you'll have copy paste a text containing your result and submit it in a gform. Thank in advance
    - french explaination text : ...
    - text input field : pseudo
    - start button :
        - when pressed, strat the test
        - save the pseudo in a variable
- test page :
    - for each image in `images/original` and `images/recolorized` in a random order : 
        - present the image for 3 seconds
        - hide the image and show 2 big buttons : original and recolorized
        - once the user clicked on one button, save it's choice and pass to the next image
    - keep a table with the resutl, for each image :
        - image_path
        - is_original : true or false
        - pseudo 
- submit result page :
    - when all images have been tested
    - title : submit your result
    - text : please click on this button to open a google form in a new tab and paste the result taht have been copy in your clipboard. Then come back to see how you'v done
    - button : copy result and go to g fom (open a gform in a new tab)
- score page :
    - once clicked on submit
    - present the result to user in a form of score like in a video game
