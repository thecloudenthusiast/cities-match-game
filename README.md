CARD MATCHING GAME

Description
A simple card flipping game where user memorizes the card in order to match the pairs. The player/user clicks the cards (which are images of famous cities) one at a time and try to match them. The cards disappear whenever a match is made until all the cards are matched- no blank image/card.

Resources
- Card matching game created with continuous deployment using AWS Code Pipeline and S3. 
- The code for the game is hosted in GitHub and an AWS S3 bucket was created with static web hosting enabled.
- AWS Code Pipeline is then used to create a continuous deployment pipeline which automatically deploys the code whenever changes are made.

Requirements
1. A GitHub account
2. An AWS Account - Free tier is okay
3. Files needed
- scripts.js     ---> Where the whole magic happens. You can edit the images to your choice. 
- style.css      ---> Styling, transition and layout
- index.html     ---> For web browser view for the game
- images 	 ---> It is recommended that you resize your images as .PNG file with 393 x 308 (height x width)

Steps to create the game
NOTE:  If you the above stated files in a GitHub repository specifically created for the game, you can skip to Step 3. 

Step 1 - Make a local repository
- Create a Folder on your desktop => name it => add your files into the folder
- alternatively, you can use VSCode to create and edit the files/images
- When everything is ready, make sure to install Git on your computer 
- Open Terminal on your VS Code and change directory (may have defaulted to it) toward that folder that you want to push there on GitHub. You can also use your desktop terminal.
- Then, execute the following commands/steps
	git init				---> This will make that folder a repository
	git add .				---> this will add ALL files to the repository
	git commit -m “Initial commit message” 	---> All files are now in the repository
	git status				---> You will get a response, “On branch master nothing to commit, working tree clean". This shows your local repository is finally created.

Step 2 - Push your files to a GitHub repository
- Go to GitHub and open your profile page
- Create a New Repository with the SAME NAME as the local repository. 
- Copy the HTTPS Key of that created repository. It can be found under the dropdown of "CODE" -green colored (clearly visible on the page).
- Go back to your VS Code terminal. Type the following commands
	git remote add origin URL_of_your_git_repo   
    e.g git remote add origin https://github.com/thecloudenthusiast/cities-match-game.git	---> This will copy/add all the files from your local to GitHub repository.
	git push origin main 
or	git push -u origin main									---> To PUSH all the files to the GitHub repository 

Step 3 - create S3 bucket in AWS
- Give a name
- Enable static website option
- Enable necessary permissions on the bucket => Turn OFF "block public access" 
- Add bucket policy 
*************************************************
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR-BUCKET-NAME/*"
            ]
        }
    ]
} 
**********************************************
- Upload your files (above)

Step 4 - Set up Code Pipeline
- Navigate and open AWS Code Pipeline
- create pipeline => 
*name it 
*pipeline type = V1
*service role = new
*execution mode = superseded
- Select the source provider - This is where you stored your input artifacts for your pipeline.
*GitHub (Version2)
*Connect to GitHub => a new page will open => create a connection name => Authorize => Install a new app =>log in to your GitHub => Only select repositories => Select the repo you created for the game => Save =>  Connect
- You should see "Ready to connect" on AWS with a green check mark
- Select your repository name
*default branch = main
*output artifact format = CodePipeline default => NEXT
- SKIP BUILD STAGE
- Choose how you deploy to instances. Choose the provider, and then provide the configuration details for that provider.
*AWS S3
*region
*bucket = the one you created on S3 for the game
*S3 object key = click on "Extract file before deploy". This will make this requirement disappear and display "Deployment path -optional"  = NEXT
- REVIEW all the configurations => CREATE PIPELINE
- After deployment, you should see the stages shown as "Succeeded" for both Source and Deploy.

Step 5 - Go back to your AWS S3 bucket
- Click properties tab => static website hosting => click on the endpoint
- A new web browser page will open automatically. The game is ready to be played

Step 6 - Making changes on GitHub to reflect in the AWS Code Pipeline
- Go to your GitHub page => click the index.html file => make some edit => commit the changes
- AWS Code pipeline will automatically detect the changes and redeploy the pipeline.
- Refresh the web browser, you should the edit.
