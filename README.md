# paleocons
Paleocons.com is a fork of the Reddit open source project.

## Install Guide

### Step 1. Install Base

Install paleocons.com auto-installer.

    $ wget https://raw.github.com/paleocons/paleocons/master/install-reddit.sh
    $ chmod +x install-reddit.sh
    $ sudo ./install-reddit.sh

You should see success message "Congratulations! reddit is now installed." Do not proceed if the installation failed with an error.

### Step 2. Set up spaces/subreddits

    $ cd $REDDIT_HOME/src/reddit/r2/r2/templates
    $ wget https://raw.github.com/reddit-archive/reddit/master/r2/r2/templates/createsubreddit.html

Now navigate to reddit.local if you are on the local network, or your domain name if you have that setup. Go to YOURDOMAIN.TLD/subreddits/create.

Set up 4 spaces/subreddits, 'general,' 'meta,' news,' and 'memes.' You will have to navigate back to YOURDOMAIN.TLD/subreddits/create every time you create one in order to create another.

Now run the following from your server once all four spaces are created

    $ rm $REDDIT_HOME/src/reddit/r2/r2/templates/createsubreddit.html
    $ cd $REDDIT_HOME/src/reddit/r2
    $ nano development.update
    
Add the following below "[DEFAULT]"

    create_sr_account_age_days = 10950

Click Ctrl+X and then Y, then run

    $ make all

Finally, set those spaces/subreddits as default by running the following in order.

    $ reddit-shell
     > from r2.models import Subreddit, LocalizedDefaultSubreddits
     > srs = [Subreddit._by_name(name) for name in ["general", "memes", "news", "meta"]]
     > LocalizedDefaultSubreddits.set_global_srs(srs)
     > exit()
    $ sudo start reddit-job-update_reddits

### Step 3. Set up admin accounts

Visit https://reddit.local or your domain and create accounts for users 'reddit,' 'automoderator,' and one for yourself..

Now run the following from your server.

    $ cd $REDDIT_HOME/src/reddit/r2
    $ nano development.update
    
At the very end of the document, click enter a few times and add a section labeled "[live_config]", like how there is a section labeled as "[DEFAULT]".

Under the new section, add the following.

    employees = %(system_user)s:admin, reddit:admin, automoderator:admin, YOURUSERNAME:admin
    
You can add more or less admin accounts than this if you would like, replace YOURUSERNAME with your username.

Then click Ctrl+X and then Y

### Final Words

Now that you've installed paleocons.com, if you've had any issues or want to learn how to complete certain tasks, visit our [FAQ](https://github.com/paleocons/paleocons/wiki/FAQ).
