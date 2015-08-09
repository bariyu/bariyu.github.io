---
layout: post
title: "The Slack Logger"
quote: Catch your bugs real time.
image: /assets/images/slack.png
video: false
dark: true
---
In this post, I will try to explain how to use <a href="https://slack.com/" target="_blank"><b>Slack</b></a> as your real time logger.

##What is Slack?
Slack is a real-time messaging application for teams and it's super efficient. Has a lot of tools and integrations which makes things a lot of easier for you and your team.

##Why use Slack as your logger?
If you are building a web server it's very important that you should log a lot things make sure everything works fine. But how often you check your logs? Is it very easy to connect to the production server get logs and look into them? If you like to code on the production server a lot and broke a lot stuff like me, knowing these bugs real-time will help you a lot. When you start using slack as your logger you will face the errors every time and probably you are gonna fix them as soon as possible.

##How to use Slack as your logger?
Slack offers a very cool <a href="https://api.slack.com/" target="blank"><b>API</b></a> so that everyone can build their own bots easily. There are already a lot Slack integrations that you can use <a href="https://slack.com/integrations" target="_blank"><b>here</b></a>. Now I am gonna talk about building your own Slack bot. What we need to here is to get a good Slack client library here or use the Slack REST api and send your <b>ERROR</b> level logs to a channel you opened for this purpose in Slack. There are a lot open source Slack client libraries that you can use. I have used <a href="https://github.com/loisaidasam/pyslack" target="_blank"><b>pyslack</b></a> for python, it is very good and sample. Also <a href="https://github.com/nlopes/slack"><b>this one </b></a> for golang. Here is a small piece of code that I use in python and flask.

```
if not app.debug:
    from pyslack import SlackHandler
    slack_handler = SlackHandler('<your_slack_token_here>', '<your_slack_channel_name>',
                                 username='server_log_bot')
    slack_handler.setLevel(logging.ERROR) #only send error level logs
    #here i am tagging my self in every post so that I receive a push or email in case I am not logged into slack :)
    slack_handler.setFormatter(Formatter(
        '@baran ```%(asctime)s %(levelname)s: %(message)s '
        '[in %(pathname)s:%(lineno)d]```'
    ))
    app.logger.addHandler(slack_handler)
```

##The Use Cases
At <a href="http://www.scorebeyond.com" target="_blank"><b>ScoreBeyond</b></a> we use the Slack logger a lot. Now I will try to explain some use cases of the Slack logger.

The first and the most expected use case is sending server errors to the Slack. Here is an example of our logger sending an error to our error channel in Slack.

![image](/assets/images/the-slack-logger/slack_logger_bot.png)

The second use case is to log your responses which takes a lot time. Because long API response times are one the things that would effect your user's experience a lot. Here is an example of that, two requests each taking around 2 minutes and 7 seconds to respond. Thanks to having the Slack logger enabled you will catch stuff like this very easily otherwise you will need the check your logs about this or receive a user complaint about this.

![image](/assets/images/the-slack-logger/slack_logger_long_response.png)

##Benefits I Get By Using The Slack Logger
1. You will directly learn your mistakes/bugs without much effort.
2. Probably the users affected with your bugs will be much and much less hence their experience will improve a lot.
3. You will become obsessed with your code and you will try to stop the errors coming into Slack because they will be distracting you a lot.
4. Your teammates seeing your errors will make you feel more responsible and you will be very motivated to fix the stuff immediately.
