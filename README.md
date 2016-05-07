# Collective Reaction Capstone

After the Spring semester of 2016, @jwward2, @ralbaya1, @hmthomas93, and @jakepruitt built a closed-loop video game that reacted to user emotions and game events. Most of the work is tightly coupled with the specific game that we built, and the database we used to collect the data. The goal of the work was to identify specific PAD patterns that correlated to user-reported good and bad events. We found, by the end of the semester, that there was a strong correlation between the angle of the <P,A> vector and the mood of the player. The player always had a better mood when the <P,A> vector had a higher angle, which was statistically significant for 755 user-reported events and 200,000 PAD events.

What we didn't do in the semester was bake this discover back into the logic of the game-adjusting reactive system. Ideally, the "brain" of our system would attempt to maximize the angle throughout the duration of gameplay, choosing the best parameters that resulted in the highest consistent average angle of the <P,A> vector. This guide will give a high-level overview to how the system works, and describe how future work on the system could improve the relatively simple proof-of-concept implementation that currently exists within the collectivereaction organization.

## Description of repositories

#### [Nightmare](https://github.com/collectivereaction/Nightmare)

**Prerequisites:** Unity, Visual Studio, and (usually) a Windows operating system

The Nightmare repository has all of the code for getting the video game up and running. It's possible to clone the repo and start the project in Unity. All of the assets will be built correctly, and the `.gitignore` file will ignore the unnecessary files. Windows was usually used for development and testing of the game, and following the walkthroughs of how to [start and use Unity](http://unity3d.com/learn) would be a good idea before working with the Nightmare game. The game will run poorly if the central emo-core server isn't running, and will be red the whole time and won't properly record damage.

#### [Emo-core](https://github.com/collectivereaction/uploader)

**Prerequisites:** Maven, Java SDK version 7 or higher, [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html), familiarity with a command line and a little experience debugging sockets

The emo-core repository is the central brain of the entire project - the part that connects the emotions to the game and the Amazon DynamoDB central database. Each of these connections is controlled on a separate thread within the emo-core program. Some things you should know beforehand are how to set up the credentials for AWS to push to the database and how to start the PAD reading programs:

1. [Download the aws cli](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) which will give you the `aws` command on the command line. Python is a prerequisite for the aws cli, so be sure you have that set up beforehand.
2. Run `$ aws configure` for your specific AWS access token and secret access token, which will allow you to push to your dynamodb tables for the game data, the pad data, and the model parameters. Follow [this guide](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html#cli-signup) for info on setting up credentials for the CLI.
3. Follow [the instructions for starting ADAS](), which you can learn in more detail from Helen, and essential comes down to starting the Emotiv headset and software and starting the JAR file programs. This will be necessary for the emo-core, which will fail with a socket error if ADAS isn't providing PA values on the socket.
4. Starting the program for emo-core requires [Maven](https://maven.apache.org/), which is an easy tool for building and executing complex java projects. Download the maven executable from [here](https://maven.apache.org/download.cgi), and set up the installation with [these instructions](https://maven.apache.org/install.html). Keep working on it until you can see something when running the `$ mvn` command on your command line.
5. In the command line, change directories to the `emo-core` folder, and run this command to compile and run the whole emo-core program: `mvn compile exec:java`.
6. If all goes well, you will get a prompt for a session ID, which will be associated with that run of the game in the database. Typically, we go with `firstname-number` to specify which player and which round of play the current session relates to.

If you have any questions, definitely ping @raid or @jakepruitt on slack or on github or twitter for help.

### [cr-cli](https://github.com/collectivereaction/cr-cli)

**Prerequisites:** Python

This tool has a few nice utilities for dealing with the PAD and game data in the dynamodb table. You can download a dump of each of the tables for analysis, which is how we created the analysis Excel document in this repo. That document is the culmination of our work on the data side from everything we collected in DynamoDB throughout the semester. Look in the docs and in the code to figure out what commands are available for handling the data.

### http://collectivereaction.github.io/dynamo-stopwatch/

**Prerequisites** Browser

This is a small web frontent that connects to our specific dynamodb database for writing good and bad events to the game event table, which helped us determine the right patterns to analyze. The table needs to be decoupled from this project, and  you most likely will not need this for the future projects, since you can optimize for the angle and not depend on subjective evaluations of emotions.
