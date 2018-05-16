---
title: A Mail.app AppleScript Rule Action
layout: post
---
I often forward email messages along to Valerie, but I usually clean the noise out of them first---a process of just enough repetitive clicking and typing to make me want to write some code. (Hey, who are we if we don't play?)

So, following the examples in <tt>/Library/Scripts/Mail Scripts/Rule Actions</tt> and with a little help from <a href="http://www.indev.ca/MailActOn.html">Mail Act-On</a>, now I just have to hit &#x2303;V:

```applescript
using terms from application "Mail"
  on perform mail action with messages theMessages
    tell application "Mail"
      repeat with eachMessage in theMessages
        set valeriesCopyOfEachMessage to make new outgoing message
        tell valeriesCopyOfEachMessage
          make new to recipient at end of to recipients with properties {name:"Valerie Todd", address:"valerie@amanikids.org"}
          set subject to "From " & (extract name from sender of eachMessage) & " -- " & subject of eachMessage
          set content to content of eachMessage
          set visible to true
        end tell
      end repeat
    end tell
  end perform mail action with messages
end using terms from
```

Dumb geek tricks.
