Source: #source/internet_resources 
Project: #project/devops 
Areas: #area/work
Subject: #subject/archtectures
Type: #type/learning
Learning priority: #priority/P3 
Status: #status/learning 
Related: [[Roadmap - Tech]]
[[Messaging-based architecture]]
[[Streaming]]

---
# Event-Driven

Imagining an example: There is man in a neighborhood that is responsible for the security. Let's think if this man is knocking on the neighbors doors asking if that is ok each 10 minutes.

That will be so boring.

To try to solve it, the process maybe changed.

The responsible of the neighborhood says:

`- If you need help, please call me!`

So, when someone in the neighborhood needs help, they call that man, and that man will run to help this neighbor.

This is a behavior driven by event. When happens something the event triggers an action.

---
## Using messaging or streaming.

The streaming or messaging tools is used also to the `Event-Driven` method.

Now imagine that there are other person interested about the neighborhood security in addition to that first security man.

At the first scenario, the security man says to call him if there's some problem, but only he will be the unique person that knows what happened.

This architecture will fail if there's a person responsible for the news and a person responsible for the interested in the same information.

Then the solution maybe a broadcast or a message panel to be activated when happens something.

The broadcast will message everyone, so the security and the news responsible were receive this. The 