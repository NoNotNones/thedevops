---
title : "SRE"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

### DevOps Foundations:  Site Reliability Engineering

#### Your job as a Devops

- SRE teams are half software engineers and half systems engineers. The real key is the engineering. It's an automation first approach that has little tolerance for repeated manual work. 
  - Continuous delivery is a DevOps approach to build engineering,
  - Infrastructure automation is a DevOps approach to systems infrastructure
  - SRE is a DevOps approach to the production support part of operations
- SRE is **“what happens when you ask a software engineer to design an operations function.”**
—Ben Traynor
![51](/thedevops/images/5-learn/5.1-sre/1.png?featherlight=false&width=40pc)

- ITSM & SRE
  - ITSM tried to solve these problems with paperwork
  - SRE tries to solve them with engineering

- One way to make an assessment of where your organization currently is on its DevOps journey is to use a maturity mode: Operations-Maturity-Model [Linkedin](https://www.linkedin.com/ambry/?x-li-ambry-ep=AQK7-TRsqg6ofQAAAY822Ivyy8YSVVaYxAsgaHw9mn08nTEDyV07pp1-JKT5HFpoI0eJFvotvhzMFP-wD48DAjK8sXX4zRQVFxz39fYzqlcfgivFp83RFTCGt8v04dGhJOfb-FjJLsuCL_vpJQ_maIWzfx5aeij8gDLp-RlOvygwwbqWVWm-Z7vAPmSbatRIziFZ5-cOU2cjERHWsDJBq3tphry16GxxwpFyFMwuCFVgrA679V40lvpiFjwXbbvIYbc2NPNYOUAxnDds2z0t_Svk7GDXhlptYOaY4BsJVUXod4bjVImrLaNaoKK-27768nuJ9R5SbiDlnZoqp5j3rJZhCwANAw4XsQ89-QhXioybu-ODBPwXgNhcDyBYL-Fo9dUrlzVH5uNZE3ktz0offVcPsGyHhyFjhPjaF5h7WGjYbsft2WhJ1sYpd5WT0hkLGTrb8t142T9srfpr3sfQcqYScn2FWaQ3DV61lxBZZQX7kt2MWVtQp0-zgMbv)

  -  We like the operations maturity model, and we've included a copy of it in the course handout   
  -  To give an example, let's look at one part of the model:
![51](/thedevops/images/5-learn/5.1-sre/2.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/3.png?featherlight=false&width=40pc)
 
  -   Revisiting it six months later gives a good scorecard for the future. And by understanding the eventual goal, you can organically improve your culture, tools, and processes. 

#### SRE Practice Areas :

#### 1. Release engineering
-  is mainly about coming up with the best, lowest friction process you can to verify and to prove changes, and to communicate their release to production that works for you and your business.

![51](/thedevops/images/5-learn/5.1-sre/4.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/5.png?featherlight=false&width=40pc)

-  That's right, continuous delivery shouldn't mean continuous disruption. People need to know what's changing and when - One great tool to do this is **Feature Flags**. Basically, as you make changes, you wrap them in toggles.
  
#### 2. Change management : 
- The goal is to have your change and release process eliminate manual work and instead rely on automation.
- The Visible Ops Handbook 
  - 1. Phase 1 – stabilize the patient, modify first res
  - 2. Phase 2 – catch and release, find fragile artifac
  - 3. Phase 3 – establish repeatable-build library
  - 4. Phase 4 – enable continuous improvement

#### 3. Seft-service automation:
- When following an SRE approach, all of this falls into the topic of self-service automation. Self-service automation is much like the title implies.Individuals inside the organization are able to do what they need to through automation. Instead of creating silos between teams, we create automation to deal with the tasks most often needed where teams interact with each other.
- There are two main benefits of creating self-service automation
    -   One of the benefits behind self-service automation is that it lowers the work in progress. 
    -   The second benefit is reliability
    
![51](/thedevops/images/5-learn/5.1-sre/6.png?featherlight=false&width=40pc)

#### 4. SLAs and SLOs 
- Service Level Agreements and Service Level Objectives.
- Service Level Agreements are the promises you make about your service to others, whether your customers, whether they're people or other services. Think of SLAs as the services contract. 
- Service level objectives are the goals that your service strives to provide in order to be deemed successful. Think of SLOs as the goals for the service.
-  Knowing what metrics to use for SLAs, and SLOs makes all the difference down the road
   - When choosing these service metrics, there are a few considerations:
     - **Frequency**:  How often is the measurement taken of your symmetric? Every 10 seconds? Every minute? 
     - **Aggregating**: over a certain time period, and averaging the measurements. Depending on the service, this can be wildly different.
     - **Geography**: Are there different goals for certain regions or geography? For example, ensuring the same service performance in different areas of the globe has widely varying costs, and architectural concerns associated with it.
     - **Monitoring system** being used to measure the metric is key. 

![51](/thedevops/images/5-learn/5.1-sre/7.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/8.png?featherlight=false&width=40pc)

#### 5. Incident management: 
Problems with production are not usually people's favorite part of the job. But problems do strike, and having a high quality playbook on how to handle them will reduce your downtime.

![51](/thedevops/images/5-learn/5.1-sre/9.png?featherlight=false&width=90pc)

-  There are tools that address these issues.
    
![51](/thedevops/images/5-learn/5.1-sre/10.png?featherlight=false&width=40pc)

- There are also open source alternatives like Cabot and Open Duty. - These tools let you set up on-call rotations and alert routing, and engineers can set up their own contact methods. Then when an alert comes in, the tools can reach out via email, text, and even voice calls to get someone to respond.
    
- Communicating the status of a problem to users is almost as important as getting someone to even fix it.  Users depend on your service, and when there's an issue, they expect to be proactively notified with clear information on how it impacts them, and when to expect a resolution. - You should have a robust incident communication process that is linked with your incident response process

![51](/thedevops/images/5-learn/5.1-sre/11.png?featherlight=false&width=40pc)
- The more you have clear, easy-to-follow incident management processes, and engineers can escalate to get the help they need, the less stressful on-call is.

#### 6. Introducting postmortems
- Once services get restored after an incident, your job is only half over. - That's right, being a **good SRE** requires learning from **failures** and **outages**.
- In a blameless postmortem, engineers share what happened and what they did, even the mistakes, without fear of punishment or retribution.
- How do we trun postmortems:
  - In a complex system there is not a single reason that a failure happened. All engineering systems are complex and you already have various safeguards in place, tests, monitoring, and so on
  - For an outage to occur, it usually requires many failures at a variety of levels. 

#### 7. The postmortem process
In our course handout, we have provided a postmortem template we like to use. **Postmortem Template.pdf**

Let's break down how we should use them. 
- First is the **description**: The description should be short, and capture the essence of the incident in just a couple of sentences.
- The **timeline** includes relevant events, including activities that happened before, during, and after the incident.
- Next, you will want to link in any **sources**: This could be customer tickets, monitoring graphs, log line snippets, or anything else relevant to documenting the outage.
- Then we have a section on **what went well**: It's easy to jump to new theories of how to make your systems more perfect, but it's also valuable to look at what safeguards are already working to help reduce frequency, impact, or length of service issues.
- The next section is **contributing causes**: one bullet point per thing that contributed to the failure happening, delayed its detection, or delayed its resolution. - Include issues with communication or process as well
- Lastly, we round this out with **corrective actions**: . What specific things can we do in the future to improve our code, systems, and our response to outages?
- Finally, we calculate the **metrics**: escribe when the beginning and to end of the incident occurred, as well as calculate time to detection and time to resolution.
- Next, we gather this to stakeholders and **hold the postmortem meeting** within a few days of the incident
- Postmortem Template sample:
  - Title :
    - [Write three-to-five-word description of the incident.]
  - Incident description:
    - [Write two or three sentences describing the incident. Make it understandable to
management and business stakeholders.]
    -  How was the incident detected? [Describe how the incident was detected. Include who, what, where, why, and how.]
    - How were customers affected? [Describe which customers were impacted and the exact impact. Include who, what,
where, why, and how. If customers reported issues, link those in here as well.]
  - Timeline
    - [Everything should be in the same time zone.]

        - Pre-incident
            • 01/01/2019 00:00:00 UTC [Add relevant information in chronological order.]
        - Incident
            • 01/01/2019 00:00:00 UTC [Add relevant information in chronological order.]
        - Post-incident
            • 01/01/2019 00:00:00 UTC [Add relevant information in chronological order.]
        - Sources
            • [Links to monitoring, dashboards, logs]

  - What went well
    - 1. [Identify things that worked well to fend off for help resolve the incident. Each should be brief.]

  - Contributing causes
    - 1. [Identify factors that caused the incident, delayed its detection, or delayed its resolution. Each should be brief.]

  - Corrective actions
    - 1. [Describe what changes have or will be taken to prevent or mitigate this or similar incidents in the future. Changes listed here include but are not limited to testing, alerting, monitoring,logging, backups, and anything else related. Doing postmortems is an evolving practice inside the organization, so changes to the postmortem process itself should be included as well. Add relevant links to these items tracked in ticketing systems.]

  - Metrics
    - When did the incident begin?
        [first errors recorded]
    - When did the incident end?
        [service restored]
    - When did we detect the incident?
        [alert fired/customer notified]
    - Time to detect
        [start_time – detect_time]
    - Time to resolve
        [start_time – end_time]

#### 8. Troubeshooting
- It's an inherent part of fixing service issues
- Troubleshooting is essentially a form of inquiry.
  - You ask a question, devise a hypothesis, construct an experiment, test, analyze data, draw a conclusion and iterate. 
![51](/thedevops/images/5-learn/5.1-sre/12.png?featherlight=false&width=40pc)
  -  Your first step is to define your problem
  -  Often you have a very preliminary, incomplete and possibly erroneous error report, maybe an alert or someone who insists the server is down.
  -  Record the exact behavior you're seeing and start asking **questions**
     -    What's the scope of the problem? 
     -    Is the problem with that entire network?
     -    All the services in that cluster?
     -    All the processes on that one server? 
     -    Is it just that endpoint? Just that user?
     -    Is it intermittent?
     -    And if so, is there a pattern to it? Has there been a change released to that service or system recently?
     -    Is there unusual traffic or are there errors on that application?
  - For each question, determine an experiment you can perform to prove or disprove it. 

- For example, if you have a web transaction that goes through six different tiers, don't jump around. Gather your data to validate each **hypothesis**
- Bring up preexisting data like monitoring and logging data and change records. Then collate the results of the experiments you performed. 
- Make sure every experiment can rule out a specific hypothesis or else it's a waste of time.
- Don't waste time debating when there's a production outage at hand. If there's a safe and easy test to rule it out, do that. 
- By logically experimenting, you can quickly get to the root of the problem.
  - First, try **experiments** that reduce the scope of the problem the most.
  - For example, if you have a web transaction that goes through six different tiers, don't jump around.  
![51](/thedevops/images/5-learn/5.1-sre/13.png?featherlight=false&width=40pc)
    - Anchor on one instrumented point and then work quickly and steadily from it so that you don't end up with information that doesn't make sense and it's hard to correlate
    - Also, prioritize non-intrusive tests over experiments that could affect the system.
    - You'll have to judge whether the risk of those experiments is worse than the problem at hand.
    - And if you can perform those experiments in a test environment, do so.
    - If you have a sufficiently production-like test environment, it's informative to know whether the issue exists there
    -  And even if it doesn't, you can reduce the impact of tests you're going to run on production by making sure they work right on the test environment and don't break anything themselves. 
    -  Of course you should know your own system and the experiments and the tools you use should be documented or better, automated to speed up the process
    -  The more parts of your system you can instrument, the easier it is to determine the exact problem. 
    -  Don't get caught up too much in finding root cause. Remember that the first job of an operator is to restore service to users. Root cause can wait.
    -  Once you've characterized a problem enough to determine a possible resolution, it's worth taking steps to save state
    -  Copy logs, take thread dumps, save copy of that VM to look at later, but then get to restoring the service by doing whatever is needed.
    -  Fixes follow the same **cycle of hypothesis**, test and analyze. But be careful, most problems aren't constant over time. It's easy to make a change that you think fixed the problem
    
 -  But for example, what really happened was a buggy script happened to finish executing right around the same time.
    -  Too often, we allow correlation to control our thinking.
    -  Scientific experimentation can't be blind. It has to be based in knowledge of your system and an understanding of why a change would have a given effect
    
 -  Here's a couple of pro troubleshooting tips.
    - First, you need to know exactly what the system looks like when it's working
    - `I've seen people waste hours running down some log error that turned out to be a normal part of that application`
    - Also in a distributed application, there are often many external dependencies, like databases, file systems, caches, other services. Knowing them and being able to quickly narrow down which are okay and which are having problems, that's one of the most time saving techniques you can have prepared.
    - And finally, it's unusual for a service disruption to be from just one cause. Often, it takes a couple factors working in concert to disrupt service
    - You know, keep that in mind because multiple variables can cause more complex and confusing symptoms. It takes even more careful experiments to discern the actual problem.
    - Remember, **troubleshooting is a science**. Don't waste time shooting from the hip. Carefully follow a rational methodology to quickly and reliably lead you to the problem and its fix.

#### 9. Performace Engineering
- Performance is one of the greatest challenges in operating a reliable system.
- Once you get the basics down, usually services aren't just all up or all down. Instead, they get slow. Pieces go down and then other parts get overwhelmed and so on.
- You need to start with a complete view of the problem. 

![51](/thedevops/images/5-learn/5.1-sre/14.png?featherlight=false&width=40pc)

- This is a reminder to always be monitoring and looking to improve the whole performance of the system
- Don't spend too much time on the parts that aren't most of the problem
- The lean adage, see the whole, is especially important to performance. You need to understand what you're optimizing and why. - Tie this to business goals or dollars as much as possible.
- Start with the most important things and the largest source of delay in those things, then you can apply all your hard-won technical know-how to that to get the most bang for your buck.
- Good monitoring instrumentation is important to help you distinguish how your systems, network and applications contribute to your overall performance.
- **Transaction tracing** is one important tool. Once found only in expensive application performance management tools, there are now open-source tools like OpenTrace and Zipkin that can trace the entire path of a given request through your system. 
- Performance engineering is where you need the most in-depth highest-resolution data.
![51](/thedevops/images/5-learn/5.1-sre/15.png?featherlight=false&width=40pc)

#### 10. Capacity and scalability
- There comes a time when you need to serve more users, more hits and more data, and you can't just make due with less. As you grow, you're faced with a challenge of scaling your systems to keep up. - And that's a more multidimensional problem
![51](/thedevops/images/5-learn/5.1-sre/16.png?featherlight=false&width=40pc)


![51](/thedevops/images/5-learn/5.1-sre/17.png?featherlight=false&width=40pc)

![51](/thedevops/images/5-learn/5.1-sre/18.png?featherlight=false&width=40pc)

- Everything's available in the cloud. I haven't used physical infrastructure since 2014. While a given cloud server is slightly more expensive than running the hardware yourself, the ability to perform short-term scaling, and the better controls you have around configuration billing and so on make for a much better capacity, and scalability story

#### 11. Distributed design
- Designing applications is a big topic that's near and dear to our hearts. - To do it right, you first need to understand that there's a difference between science and engineering.
- These are 12 factors that make for a successful distributed application, including how to manage external dependencies, how to store state, and how to scale.

![51](/thedevops/images/5-learn/5.1-sre/19.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/20.png?featherlight=false&width=40pc)
- Here's one example. Take a service that starts to experience timeouts from one of its backends
  - The requests keep coming in, and the service has more and more slow pending requests until it finally gets overloaded and it crashes.
  - Or even worse, you detect that slow performance and deliberately scale up your front end tier as a result, which puts even more load on the backend, virtually guaranteeing that it'll be overloaded and crash even more quickly
  - This deliberately returns errors quickly if it's backend goes over a certain error level to avoid failure.
- That's right. **Observability** means that your service is clearly presenting what's going on with it. In other words, it's logging useful things, and emitting metrics, and maybe has a status endpoint that expresses what's going on with it. 
- Exactly. Then controllability is your ability to control the state of your service. - Being able to pause and resume your apps processing, or turn features on and off with feature flags at runtime

#### 12. Deliberate adversity
- If it hurts, do it more often. - You sound like that annoying guy at the gym. Can we just skip this one? - No way. While yes, the proverbial wisdom is true for building muscle, it's also true for systems
- In 2011, Netflix captured the DevOps world's attention when they released the Chaos Monkey
  - I remember that it was a really radical approach at the time.
  - Their goal was to improve service stability and increase fault tolerance by injecting failure into the system. The thinking here was pretty simple.
    - Netflix allowed developers to have autonomy in architecting and building services however they wanted.
    -  So to make sure that they were building services and applications that could handle failures from the cloud provider, they would randomly delete services during normal business hours.
  - This has become part of the Netflix culture, and they've released an entire genre of open-source tooling around this that they brand as the Simian Army. 
  - Other tools in the Simian Army include Chaos Gorilla, which will terminate entire Amazon region, and Latency Monkey, which adds in artificial delays to simulate service degradation
  - These tools function as adding deliberate chaos and adversity into the systems. These tools force developers to create services that can withstand this adversity. 
  - One of the recommended ways to do this is to think in terms of chaos experiments. - You make a hypothesis, and then take observations to prove whether your hypothesis was correct or not. - Yeah, you could easily call what Chaos Monkey did a chaos experiment

- **Example**:  I remember once working on a project, and we were building an API that accepted design files, and then they would get processed on the back end. Most of these files were pretty small. So I asked the developer, hey, what if I sent you a two gigabyte file? - Oh no. - Yeah, we did it, you know, and I was able to crash the whole service as it caused a cascading failure, where one node would fail, and then the next node would pick it up and then that would fail, and then the next. We found several bugs that day and implemented a circuit breaker pattern. **Chaos engineering** is best done in this fashion of experimentation. 

- Chaos engineering is a new discipline, and it's one that can really pay off. - Right, you know, how often does Netflix go down? It's not that often. - Being mean to your code and doing hard things more often actually does make your service more resilient and more reliable for your customers.

#### SRE Organizaion
#### 1. Organizing SREs

-  Remember Conway's law, how you organize definitely has an impact on how your systems turn out.
![51](/thedevops/images/5-learn/5.1-sre/21.png?featherlight=false&width=40pc)

- Here's some models we've seen work to organize for this approach. At one company, we actually tried a couple different models.
  - So we started with the traditional separate ops team model. It's very hard to break traditional expectations while maintaining this model. 
![51](/thedevops/images/5-learn/5.1-sre/24.png?featherlight=false&width=40pc)
  - People love to assign work tickets to the group and this organizational silo also forms a barrier to collaboration. And it's probably even harder to break the habits of operation engineers without a change. 
![51](/thedevops/images/5-learn/5.1-sre/22.png?featherlight=false&width=40pc)
  - So then we declared the death of the ops team and changed to embedding operations engineers into every product team but still reporting up to a central manager. This lets the team work at its own pace without being blocked on a central team and injects them with some operational knowledge.
![51](/thedevops/images/5-learn/5.1-sre/23.png?featherlight=false&width=40pc)

- So which setup is best? - That's a trick question. There's no single best. There's what works best given the constraints that you have. But don't artificially limit yourself. You can iterate towards a more successful model no matter where you start out originally

#### 2. The softer side of SRE
The softer side of SRE
- Time Management for System Administrators by Thomas Limoncelli –
http://shop.oreilly.com/product/9780596007836.do

- Pragmatic Thinking and Learning by Andy Hunt –
https://pragprog.com/book/ahptl/pragmatic-thinking-and-learning

#### Conclusion

Learn links: https://www.linkedin.com/learning/devops-foundations-site-reliability-engineering/the-softer-side-of-sre-14766859?resume=false&u=103729754

Release It! by Michael Nygard – https://pragprog.com/book/mnee2/release-it-second-edition

- Other links: https://pragprog.com/titles/mnee2/release-it-second-edition/

Site Reliability Engineering: https://sre.google/books/

![51](/thedevops/images/5-learn/5.1-sre/25.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/26.png?featherlight=false&width=40pc)
![51](/thedevops/images/5-learn/5.1-sre/27.png?featherlight=false&width=40pc)