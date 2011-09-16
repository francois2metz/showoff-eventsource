!SLIDE
# EventSource
## François de Metz
## af83

!SLIDE small
# The (big) problem
## You want to show latest tweets tagged #af83 on your website

!SLIDE code smaller

    @@@ javascript
    $.jsonp({
        url: "http://search.twitter.com/search.json?q=af83s&rpp=8",
        dataType: "jsonp",
        callbackParameter: "callback",
        success: function(result) {
           // code here
        },
        error: function(XHR, textStatus, errorThrown) {
           // code here
        }
    })

!SLIDE
# It works :/

!SLIDE
# But you would like something much simpler

!SLIDE code smaller

    @@@ javascript
    var url = "http://search.twitter.com/search.json?q=af83&rpp=8"
    
    var twitter = new EventSource(url)
    
    twitter.onmessage = function(e) {
        console.log(e.data)
    }

!SLIDE
# It doesn't works

!SLIDE small
# Twitter doesn't provide an EventSource compatible API

!SLIDE
# And ...

!SLIDE
# Hu?

!SLIDE
# Cross domain requests?

!SLIDE
# CORS anyone?

!SLIDE bullets incremental
# NO.

* https://bugzilla.mozilla.org/show_bug.cgi?id=664179
* https://bugs.webkit.org/show_bug.cgi?id=61862

!SLIDE bullets incremental small
# So what?

* http based
* no *Upgrade* header
* no tcp low-level
* not your home made protocol which is better because you have specific requirement...
* fallbacks in javascripts (long polling, COMET, ... you know that)

!SLIDE
# Show me

!SLIDE code

    HTTP1/1 200 OK
    Content-Type: text/event-stream

    data: my message
                           <-- this a new line
    data: a new one

---

    @@@ javascript
    source.onmessage(function(event) {})

!SLIDE code

    event: delete          <-- this the event name
    data: a plop message
    
---

    @@@ javascript
    source.addEventListener('delete', 
                            function(event) {})

!SLIDE

    retry: 5
    
    id: 5943859389038535 <- used on reconnection
    data: my message

---

    @@@ javascript
    source.onmessage(function(event) {
        console.log(event.lastEventId)
    })

!SLIDE bullets
# Other solutions

* websockets (birectional stream)
* longpolling

!SLIDE
# Use it in ruby!
# em-eventsource

## EventMachine + em-http-request
## https://github.com/AF83/em-eventsource

!SLIDE small
# EventSource
## Use it
## http://dev.w3.org/html5/eventsource/
# Questions ?
