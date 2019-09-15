# e-learning-bypass

Bypass e-learning progress check.


## Usage

Just type this in a javascript console in your browser.

~~~javascript
track_for_onwindow(99, jwplayer().getDuration() + 1);
~~~


## How this works

The e-learning site provides movie playback with progress checking.

Once the page is loaded, it start reporting the learning progress to the server.

At this point we can manipulate the report procedure by calling the *report function* with designated parameters.

### The function

That function is `track_for_onwindow`.

~~~javascript
function track_for_onwindow(state, position) {

    if(is_progress) {
        $.ajax( {
             url: wwwroot + "/mod/vod/action.php",
             data: {
                 type: 'vod_track_for_onwindow',
                 track: track,
                 state : state,
                 position : position,
                 attempts : attempts,
                 interval : 60						
             },
             type: "POST",
             success: function(data) {
                if(data.code == mesg.failure) {
                    alert(data.msg);
                    return false;
                }
            },
            error : function(xhr, msg) {
                if(xhr.responseText) {
                    alert(msg + " : " + xhr.responseText);
                }
            }
        });
    }
}
~~~

We can call it anytime with any parameters.

Parameters are like below:

### Parameters

The function needs two following arguments.

#### state

A state of movie playback.

It could be one of these:
- 3: start
- 5: complete
- 99: update or finish

#### position

Movie position to report.

It is recommended to be greater or equal to zero, and lesser or equal to jwplayer().getDuration() + 1.

As stated in a comment by original author of the site, there are some cases where jwplayer().getPosition() returns one less than the jwplayer().getDuration() at the end of the playback.

Therefore jwplayer().getDuration() + 1 would be proper if you want to mark the lecture to be completely watched.

If the position passed to the function exceeds normal range, it will occur *UNRECOVERABL* server database error.

So be careful when you type.


## Disclaimer

**IMPORTANT**

The author is not responsible for any damage or loss of property to any person following this guide.

This does not do your study for you.

Your study, your score, your time, they all are on you.

This only gives you another chance to put off what you needed to do today for a while.
