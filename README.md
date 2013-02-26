getthereintime
==============

Description
----------------------------

* aware of the location of the calendar owner
* calculate the distance from the user's current location to the next appointment and, based on that distance, alerts the user of the latest time to leave in order to get to the appointment
* use some sort of GIS API to calculate the distances
* integrate with an already existing calendar system
* allow the user to save commonly used locations as part of his/her profile

Some things to note
----------------------------

If we write this in Python, it cannot be a mobile app - the resources don't really exist for that yet.

So for now it would have to be a web app that is optimized for mobile viewing, but there's no reason not to prototype this as a web application, work out the logic and API interactions, and then convert it to mobile in another language at a later date.

As a web app, it can't integrate with a phone's push notification system - alerts will have to be done via email (or possibly via a pop-up an alert if the app is open in the user's browser at the time an alert is scheduled to be sent).


What does the user flow look like?
----------------------------

I'll leave the discussion of the finer points up to all of you, but I'll make some suggestions:

1. The home page should include a description of how the app works (for example, let the user know that he/she will need a Google calendar)
2. User creates an account and logs in
3. User connects account to calendar, allowing app permissions to read via calendar API
4. User adds locations that can be bookmarked to his/her profile
5. User configures notifications

Aside from these points, I don't think users will have much interaction with the app as a web site - most of the interaction the user will have will concern locations and notifications - so only a few pages will need to be built (home, registration, profile, notifications, etc.).

Modules
----------------------------

I'm going to recommend using the Django framework for building this project for a few reasons:

1. Django comes with a lot of built-in services that we can use, such as signals, user authentication, and email.
2. There are a lot of already-built open source Django applications out there that we can integrate into our own project and modify as necessary.
3. Django's project structure is very modular, so it will be relatively easy to divide the labor and have everyone working on the parts of the app they're most interested in.
4. A lot of you are already working with Django over at UT - this can only help you get more experience.

Here are the basic modules I think you'll need:

### Registration

> Registration isn't built into Django.  It should be fairly simple to build from scratch, but there are also a couple of popular modules out there that a lot of developers use:
> 
> https://bitbucket.org/ubernostrum/django-registration
> https://pypi.python.org/pypi/django-inspectional-registration

### Profile

> This is another common module for which there's already a good solution out there:
> 
> https://bitbucket.org/ubernostrum/django-profiles/
> 
> It would need to be modified to:
> * store the user's current location
>   * determine the user's current location (need to determine at what intervals)
>     OR
>   * let the user set a current location manually
> * store the user's "next event" (see below - this could just as easily be a model in the calendar app)

### Calendar

> * Integrate with Google calendar, or the common calendar API of your choice
> * Poll the calendar for the next upcoming event (need to determine at what intervals)
> * Write an algorithm to calculate geographical distance and drive time, based on map APIs (and traffic, if available)**
> 
> Some suggestions for handling the event flow:
> * When polling for the next event, calculate the geographical distance and set an alert time, then store the whole thing as the user's "next event"
> * Replace the "next event" if a new calendar event is added that predates it

APIs
----------------------------

Google has simple APIs for both maps and calendars, but I'd suggest doing some research and seeing if there are any others out there you'd rather use:

https://developers.google.com/maps/
https://developers.google.com/google-apps/calendar/

I haven't used the map API much, so I can't say definitively that it's the best solution for calculating distances and drive times - the GIS specialists among you should look into this.  :) 

Google apparently does not have a traffic API, but Bing, MapQuest and a few other services do - maybe there's a way to integrate one of them?:

http://msdn.microsoft.com/en-us/library/hh441725
http://developer.mapquest.com/web/products/dev-services/traffic-ws

Finding a good traffic API that's open source, free, and easy to use would be a great research project for someone.

Other possibilities:

I could see this application going beyond email alerts - using a service like Twilio, for example, to make phone calls to the user, or sending tweets as reminders.  On that note, I found this section on Code Academy with resources/tutorials for getting started with some of the more popular APIs out there:

http://www.codecademy.com/tracks/apis


