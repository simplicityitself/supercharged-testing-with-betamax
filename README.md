# Supercharged Microservice Testing with Betamax

*Yawn*. That pretty much sums up doing functional testing for microservices where you're pulling data feeds from an external system. No matter how fast you'd like things to go, you're at the mercy of network latency and the responsiveness of the source system to get you enough data to feed into your microservices pipeline to give you the confidence that it works properly.

If you find your own life dwindling away as you run YAFT (yet another functional test) against an HTTP external source, you're not alone. This problem is not going away as microservice pipelines provide functional composition in support of mashing up data from disparate sources. 

So what can be done to quite literal save your life in these situations? On the one hand there's a nice timeslot for coffee, a short chat and possibly a hike across the Sahara (including pre-training time). On the other hand, there's [*Betamax*](http://freeside.co/betamax/).

## A (Brief) Introduction to Betamax

Betamax is a really simple system for recording HTTP interactions and then playing them back, usually in testing scenarios.

Adopting what is effectively a proxy man-in-the-middle position,  Betamax records detected HTTP interactions to a *tape* as shown below:

TBD Diagram showing Betamax as a man-in-the-middle recording HTTP interations to a Betamax tape.

The Betamax tape is a human-readable [YAML](http://www.yaml.org) file that you can amend if you choose to do so. The main purpose of the tape is to then be automatically used in place of the full, and slow, interactions the next time the test is run.

TBD diagram showing the tape being used for replay.

## The Challenge: HTTPS

I came to Betamax as someone who was running a long-winded test against an external chat-style system. My micro service's single-purpose was to retrieve 1-hour batches of conversations in the chat system, and ferrying that on to the next micro service in the chain for further processing. 

> *NOTE:* This blog is about a real 'live' system that Simplicity Itself are working on so I've had to remove the names to protect the *innocent*.

Architecturally, my micro service's place in the world was fairly straightforward:

TBD Diagram of where my micro service sits in the world.

My micro service is built using [Spring Boot](http://projects.spring.io/spring-boot/), mainly because ideally the project is eventually handed over to the client's own Java and Spring-centric development teams. The main purpose of the project is to provide the required functionality, the secondary goal being to demonstrate [microservices and an antifragile software system](https://leanpub.com/antifragilesoftware).

Ok, first let's take a look at the test before introducing Betamax. This test's aim was to prove that a few hours of posts, numbering roughly several thousands of posts, could be effectively slurped into the pipeline in nicely bounded hour chunks. For this testing, I used (as I often do when working with Java) the excellent [Spock framework](https://code.google.com/p/spock/):

	class HourBatchChatImportServiceIntegrationSpec extends BaseLocalEnvironmentSpec {

  		def "import and process chatter feed items correctly"() {

    		given:
    		def hourBatchChatImportPipeline = applicationContext.getBean(HourBatchChatImportService)
    		def calculateDateTimeRange = applicationContext.getBean(CalculateDateTimeRange)

    		def from = "2014-05-22-1"
    		def to = "2014-05-22-5"
    		def (start, end) = calculateDateTimeRange.getStartEnd(from, to)

    		when:
    		def response = hourBatchChatImportService.call(start, end)

    		then:
    		response != null
    	}
	}

The test grabs the hourBatchChatImportService, calculates a date range for the posts to be imported (in this case from 22nd May at 1am to 22nd May at 5am inclusive) and then kicks off the pipeline passing in those start and end date range parameters.

I'm also using a `BaseLocalEnvironmentSpec` to set up the Spring application context, switching on a 'local' profile that selects downstream microservices running on my local testing machine (rather than resolving them to full services as happens at runtime). The contents of the `BaseLocalEnvironmentSpec` is shown below:

	abstract class BaseLocalEnvironmentSpec extends Specification {

  		def static applicationContext;

  		def setupSpec() {
    		applicationContext = new AnnotationConfigApplicationContext()
    		applicationContext.getEnvironment().setActiveProfiles("local", "test");
    		applicationContext.register(ChatHooverController);
    		applicationContext.refresh()
  		}
	}

> The tests shown here are using Spock but there is also support for JUnit within Betamax. We'll focus on using Spock for this blog as the tests are much simpler, clearer and therefore more readable and comprehendible.

The problem was, when running this test things took a *long* time. Fair longer than anyone was going to be willing to wait and so the test was in serious danger of being one that was rarely run.

I was willing to suck up that time once ,or maybe even twice, but *every time* the test is run? No way.

Enter Betamax, stage left.

## Introducing Betamax to the Chat Service's Testing



The source was an HTTP interface 



Further Information

* Book
* Course on Simplicity Itself

 




