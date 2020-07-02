# The Smallest Starting Point

So, you want to build a full-stack JavaScript application with:

- An Express web server
- A PostgreSQL database
- A React front-end

And you want it to work locally as well as be easy to deploy?

We've got your back:

## Local Development

### Setting Up

First, fork this repo by clicking the "Fork" button in the upper right corner.

Within your forked repo, clone the repo to your local computer:

- Click the green "Code" button and copy the provided URL
- From the terminal, in your curriculum folder, enter `git clone https://github.com/YOUR-NAME/grace-shopper.git` and replace the URL with the one you just copied

#### Configuring a Remote for Your Fork

You need to create a link between your forked repo and the original repo in order to be able to pull down merged changes from the original repo.

- Copy and paste the following into your terminal (do not change the URL this time)

```bash
git remote add upstream https://github.com/jendiorio/grace-shopper.git
```

- To confirm that it worked, enter `git remote -v` in your terminal - you should see 4 entries total, two starting with `origin` and two starting with `upstream`

#### Completing Setup

Then, run `npm install` to install all node modules.

Run `createdb donut-hole` from your command line to create the local testing database on your computer.

Finally you can run `npm run server:dev` to start the web server.

In a second terminal navigate back to the local repo and run `npm run client:dev` to start the react server.

This is set up to run on a proxy, so that you can make calls back to your `api` without needing absolute paths. You can instead `axios.get('/api/posts')` or whatever without needing to know the root URL.

Once both dev commands are running, you can start developing... the server restarts thanks to `nodemon`, and the client restarts thanks to `react-scripts`.

### Working with Git

#### Commit and Push Your Code

**Important:** Make small and frequent pull requests.

The first step is commiting your code:

```bash
git add .
git commit -m "commit message"
git push origin master
```

The final command above will push the code to your forked git repo.

#### Creating a Pull Request

- In a browser, go to your forked repo in GitHub
- Click on the "Pull requests" tab
- Click the green button for "New pull request"
- On the left side, it should default to `base repository: jendiorio/grace-shopper` and `base: master` (if not, change it). The right side will be your repo and master, which you shouldn't have to change.
- Click the green "Create pull request" button
- The title should clearly represent the changes you are submitting
- Add an optional comment
- Click "Create pull request" button
- Let your team know there is a pull request to review by copying/pasting the pull request URL into our slack group

#### Reviewing a Pull Request (Approving Changes)

New code will not be merged in until approved by at least one team member. Pull requests cannot be approved by the person submitting the new code.

- Open the pull request URL and click the "Files changed" tab
- To Reject Code: If changes are needed before the code can be approved, add a comment explaining what changes should be made. There are two ways to add comments:
  - Hover over a specific line of code and click the + button, enter your comment, and click "Add single comment" (repeat for any other comments you want to add)
  - If you want to leave a general comment, click the green "Review changes" button, enter your comment, then click "Submit review"
- To Approve Code: If no changes are needed, click the green "Review changes" button and choose "Approve", then click the "Submit review" button
  - On the following screen, click the green "Merge pull request" button

#### Keeping in Sync

After new code is merged from a pull request (as described above), the team members that did not submit the new code will need to pull down the lastest changes so we are all working from the most recent version of the code.

To do so, run the following command:

```bash
git merge upstream/master
```

Then, to keep your GitHub repo (fork) in sync, run the following command:

```bash
git push origin master
```

### Project Structure

```bash
├── db
│   ├── index.js
│   └── init_db.js
├── index.js
├── package.json
├── public
│   └── index.html
├── routes
│   └── index.js
└── src
    ├── api
    │   └── index.js
    ├── components
    │   ├── App.js
    │   └── index.js
    └── index.js
```

Top level `index.js` is your Express Server. This should be responsible for setting up your API, starting your server, and connecting to your database.

Inside `/db` you have `index.js` which is responsible for creating all of your database connection functions, and `init_db.js` which should be run when you need to rebuild your tables and seed data.

Inside `/routes` you have `index.js` which is responsible for building the `apiRouter`, which is attached in the express server. This will build all routes that your React application will use to send/receive data via JSON.

Lastly `/public` and `/src` are the two puzzle pieces for your React front-end. `/public` contains any static files necessary for your front-end. This can include images, a favicon, and most importantly the `index.html` that is the root of your React application.

### Command Line Tools

In addition to `client:dev` and `server:dev`, you have access to `db:build` which (you will write to) rebuilds the database, all the tables, and ensures that there is meaningful data present.

## Deployment

### Setting up Heroku (once)

```bash
heroku create hopeful-project-name

heroku addons:create heroku-postgresql:hobby-dev
```

This creates a heroku project which will live at https://hopeful-project-name.herokuapp.com (note, you should change this to be relevant to your project).

It will also create a postgres database for you, on the free tier.

### Deploying

Once you've built the front-end you're ready to deploy, simply run `git push heroku master`. Note, your git has to be clean for this to work (which is why our two git commands live as part of getting ready to deploy, above).

This will send off the new code to heroku, will install the node modules on their server, and will run `npm start`, starting up your express server.

If you need to rebuild your database on heroku, you can do so right now with this command:

```bash
heroku run npm run db:build
```

Which will run `npm run db:build` on the heroku server.

Once that command runs, you can type `heroku open` to get a browser to open up locally with your full-stack application running remotely.
