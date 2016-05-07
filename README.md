# Collective Reaction Capstone

After the Spring semester of 2016, @jwward2, @ralbaya1, @hmthomas93, and @jakepruitt built a closed-loop video game that reacted to user emotions and game events. Most of the work is tightly coupled with the specific game that we built, and the database we used to collect the data. The goal of the work was to identify specific PAD patterns that correlated to user-reported good and bad events. We found, by the end of the semester, that there was a strong correlation between the angle of the <P,A> vector and the mood of the player. The player always had a better mood when the <P,A> vector had a higher angle, which was statistically significant for 755 user-reported events and 200,000 PAD events.

What we didn't do in the semester was bake this discover back into the logic of the game-adjusting reactive system. Ideally, the "brain" of our system would attempt to maximize the angle throughout the duration of gameplay, choosing the best parameters that resulted in the highest consistent average angle of the <P,A> vector. This guide will give a high-level overview to how the system works, and describe how future work on the system could improve the relatively simple proof-of-concept implementation that currently exists within the collectivereaction organization.

## Description of repositories

#### [Nightmare](https://github.com/collectivereaction/Nightmare)

**Prerequisites:** Unity, Visual Studio, and (usually) a Windows operating system

The Nightmare repository has all of the code for getting the video game up and running. It's possible to clone the repo and start the project in Unity. All of the assets will be built correctly, and the `.gitignore` file will ignore the unnecessary files. Windows was usually used for development and testing of the game, and following the walkthroughs of how to [start and use Unity](http://unity3d.com/learn) would be a good idea before working with the Nightmare game. The game will work okay if the central emo-core server isn't running, but will be red the whole time and won't properly record damage.

#### [Emo-core](https://github.com/collectivereaction/uploader)

**Prerequisites:** Maven, Java SDK version 7 or higher, [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html), familiarity with a command line and a little experience debugging sockets

The emo-core repository is the central brain of the entire project - the part that connects the emotions to the game and the Amazon DynamoDB central database. Each of these connections is controlled on a separate thread within the emo-core program. Some things you should know beforehand are how to set up the credentials for AWS to push to the database and how to start the PAD reading programs.

