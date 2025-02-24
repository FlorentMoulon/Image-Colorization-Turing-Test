Here is a webpage using HTML, CSS and JS that allow researcher to pass turing test on image to people.


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



# code :
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Colorization Turing Test</title>
    <style>
        .page {
            display: none;
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
            font-family: Arial, sans-serif;
        }
        .active-page {
            display: block;
        }
        #imageContainer {
            text-align: center;
            margin: 20px 0;
        }
        #testImage {
            width: 100px;
            height: 100px;
            border: 2px solid #333;
        }
        .choice-buttons {
            display: none;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }
        button {
            padding: 15px 30px;
            font-size: 18px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
        }
        #originalBtn {
            background-color: #4CAF50;
            color: white;
        }
        #recolorizedBtn {
            background-color: #f44336;
            color: white;
        }
        #scoreResult {
            margin-top: 20px;
            padding: 15px;
            background-color: #f0f0f0;
            border-radius: 5px;
        }
        .result-item {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
        }
        .correct {
            background-color: #d4edda;
            border: 1px solid #c3e6cb;
        }
        .incorrect {
            background-color: #f8d7da;
            border: 1px solid #f5c6cb;
        }
        .result-item img {
            width: 50px;
            height: 50px;
            margin-right: 10px;
            vertical-align: middle;
        }
    </style>
</head>
<body>
    <!-- Home Page -->
    <div id="homePage" class="page active-page">
        <h1>Image Colorization Turing Test</h1>
        <div class="english">
            <p>You will be presented N images of 100*100 pixel, some of them are original pictures, some are grayscale images colorized using an AI program. You will have 3s to look at an image and then you'll have to say if it was original or recolorized. At end, you'll have to copy-paste a text containing your result and submit it in a Google Form. Thank in advance!</p>
        </div>
        <div class="french">
            <p>[French translation of the above text]</p>
        </div>
        <input type="text" id="pseudo" placeholder="Enter your pseudo">
        <button onclick="startTest()">Start Test</button>
    </div>

    <!-- Test Page -->
    <div id="testPage" class="page">
        <div id="imageContainer">
            <img id="testImage" src="" alt="Test image">
        </div>
        <div class="choice-buttons">
            <button id="originalBtn">Original</button>
            <button id="recolorizedBtn">Recolorized</button>
        </div>
    </div>

    <!-- Submit Page -->
    <div id="submitPage" class="page">
        <h2>Submit Your Result</h2>
        <p>Please click the button below to open the Google Form in a new tab and paste the copied results. Then come back to see your score.</p>
        <button onclick="copyResults()">Copy Results and Open Form</button>
    </div>

    <!-- Score Page -->
    <div id="scorePage" class="page">
        <h2>Your Results</h2>
        <div id="scoreResult"></div>
    </div>

    <script>
        // Sample images - replace with actual image filenames
        const originalImages = ['img1.jpg', 'img2.jpg'];
        const recolorizedImages = ['img3.jpg', 'img4.jpg'];
        
        let testImages = [];
        let currentImageIndex = 0;
        let results = [];
        let userPseudo = '';

        function initializeTest() {
            // Create combined array with original/recolorized flags
            testImages = [
                ...originalImages.map(img => ({ path: `images/original/${img}`, is_original: true })),
                ...recolorizedImages.map(img => ({ path: `images/recolorized/${img}`, is_original: false }))
            ];
            
            // Shuffle the images
            testImages = testImages.sort(() => Math.random() - 0.5);
        }

        async function startTest() {
            userPseudo = document.getElementById('pseudo').value || 'anonymous';
            initializeTest();
            switchPage('testPage');
            
            for (currentImageIndex = 0; currentImageIndex < testImages.length; currentImageIndex++) {
                const image = testImages[currentImageIndex];
                
                // Show image
                document.getElementById('testImage').src = image.path;
                document.getElementById('testImage').style.display = 'block';
                await wait(3000);
                
                // Hide image and show choice buttons
                document.getElementById('testImage').style.display = 'none';
                document.querySelector('.choice-buttons').style.display = 'flex';
                
                // Wait for user choice
                const choice = await getUserChoice();
                
                // Record result
                results.push({
                    image_path: image.path,
                    is_original: image.is_original,
                    pseudo: userPseudo,
                    user_choice: choice,
                    correct: (image.is_original && choice === 'original') || (!image.is_original && choice === 'recolorized')
                });
                
                // Hide buttons and clear image
                document.querySelector('.choice-buttons').style.display = 'none';
                document.getElementById('testImage').src = '';
                await wait(500);
            }
            
            switchPage('submitPage');
        }

        function getUserChoice() {
            return new Promise(resolve => {
                const originalBtn = document.getElementById('originalBtn');
                const recolorizedBtn = document.getElementById('recolorizedBtn');
                
                const handleChoice = choice => {
                    originalBtn.removeEventListener('click', originalHandler);
                    recolorizedBtn.removeEventListener('click', recolorizedHandler);
                    resolve(choice);
                };
                
                const originalHandler = () => handleChoice('original');
                const recolorizedHandler = () => handleChoice('recolorized');
                
                originalBtn.addEventListener('click', originalHandler);
                recolorizedBtn.addEventListener('click', recolorizedHandler);
            });
        }

        function copyResults() {
            const resultJSON = JSON.stringify(results, null, 2);
            navigator.clipboard.writeText(resultJSON).then(() => {
                window.open('https://docs.google.com/forms/d/your-form-id', '_blank');
                showScore();
            });
        }

        function showScore() {
            switchPage('scorePage');
            const correct = results.filter(r => r.correct).length;
            const total = results.length;
            const accuracy = (correct / total * 100).toFixed(1);

            let scoreHTML = `
                <p>Correct answers: ${correct}/${total}</p>
                <p>Accuracy: ${accuracy}%</p>
                <h3>Detailed Results:</h3>
            `;

            results.forEach(result => {
                const resultClass = result.correct ? 'correct' : 'incorrect';
                const resultText = result.correct ? 'Correct' : 'Incorrect';
                const correctAnswer = result.is_original ? 'Original' : 'Recolorized';
                scoreHTML += `
                    <div class="result-item ${resultClass}">
                        <img src="${result.image_path}" alt="Result image">
                        <span>Your choice: ${result.user_choice} | Correct answer: ${correctAnswer} (${resultText})</span>
                    </div>
                `;
            });

            document.getElementById('scoreResult').innerHTML = scoreHTML;
        }

        function switchPage(pageId) {
            document.querySelectorAll('.page').forEach(page => {
                page.classList.remove('active-page');
            });
            document.getElementById(pageId).classList.add('active-page');
        }

        function wait(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }
    </script>
</body>
</html>



# Your task :
Upgrade the style and CSS. I want the UI to be minimalist and clear.

Use this color palette :
black : 112D3F
accent : 22577A
accent 2 : 80ED99
recolorized images : 38A3A5
original images : 57CC99
white : FBFBF2

You can take inspiration from duolingo or kahoot.
Make all the text bigger and lisible
Use balance and center image when possible.