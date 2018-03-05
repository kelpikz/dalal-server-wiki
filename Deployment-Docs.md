Deployment is mostly as described in README of dalal-street-server repo.

**Couple of extra steps:**

We have two bot accounts - one for travis (**dalal-street-travis-bot-2**) and one for deployment (**dalal-street-testserver-bot**). The testserver bot is used for deployment on both the test server and on the main server. The bots have been added as contributors to the repo. Both bots use ssh-keys to get access to the repos.

The credentials have been emailed to coderick14's gmail account (Deep).

**Travis**:
My (thakkarparth007's) travis account has been configured to use the private key of dalal-street-travis-bot-2. While changing the travis account, make sure to add the private key to it.

**Deployment**:
The ssh-keys need to be added to ~/.ssh folder as id_rsa and id_rsa.pub to make things automatically work, otherwise you'll have to do `eval $(ssh-agent -s); ssh-add ~/.ssh/<the_private_key_name>`.

**Auto deploy**:
There was a python auto_deploy script that I used to push master's changes to the test server automatically, but unfortunately we didn't have a copy of it. It's a simple python http server that gets webhook requests from github, and it restarts docker-compose process to redeploy. Won't be hard to rewrite. Make sure to add the webhook secret properly in the settings.

**Adding new bot account**
For whatever reason if you want to add a new bot user, make sure you set the email of the bot user like this: <your_email>+your_bot_name@gmail.com - gmail ignores everything after the +, but github doesn't. This will allow you to reset the password easily because the bot's emails will go to your account directly.