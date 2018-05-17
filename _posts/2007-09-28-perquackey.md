---
title: Perquackey
layout: post
---
<p class="update"><strong>Update:</strong> The <a href="http://perquackey.matthewtodd.org/">Perquackey site</a> works a lot better now---I've just switched to a <a href="http://personal.riverusers.com/~thegrendel/software.html" title="YAWL">better word list</a> and made the <a href="http://matthewtodd.org/svn/public/perquackey/lib/perquackey/fast_dictionary.rb">dictionary lookup</a> much faster!</p>

<img src="/images/2007/09/28/perquackey.jpg" width="500" height="375" alt="Perquackey" style="border: 1px solid #333" />

Valerie's parents were here visiting for a few weeks, during which time her mom and I played Perquackey every chance we got.

<a href="http://www.cardinalgames.com/instruct/perquacky.htm">Perquackey</a>, pictured above, is a word-spelling game like Boggle, except word length matters, so you take more time to think.

<p class="update"><strong>Update:</strong> My bad. <a href="http://www.mindspring.com/~alanh/">Alan Hensel</a> points out that word length does matter in <a href="http://en.wikipedia.org/wiki/Boggle">Boggle</a>. What's additionally true about Perquackey is that it <em>limits</em> you to spelling just 5 words of a given length.</p>

<h2>Smack</h2>

Which, apparently, gives you more time for trash-talking. Weeks before their arrival, I started getting gems like this in my inbox:

> Perquackey players like you don't grow on trees. They swing from them.

And, well, you just can't let something like that slide. After the inevitable escalation, we'd raised the medium to an art form:

> In the great wild Serengett,  
> A dik dik grazes, 'till he's met in  
> Millisecond lightning flash,  
> Tim-tumbled over, turned to  
> Hash.
>
> Eaten live by  
> Powerful beast,
>
> Eviscerated  
> Raw, deceased.
>
> Quite the scene  
> Under the sun,  
> Although another "death" shall  
> Come, when self-appointed Queen Per-  
> Kathy, unsuspecting,  
> Minces daftly,  
> Aligning letters one by one,  
> Slowly, surely, not much fun.
>
> Then sit I in to take my turn  
> Egads! How did this kid here learn?  
> Round one completed, so the game,  
> Your skills shown up to be quite lame.
>
> Ouch.

The alert reader will note the first letters of each line spell out "I am the perquackmaster, yo."

<h2>Style</h2>

Sadly, though, age and treachery won out over youth and intelligence. But I held my own spelling <em>interesting</em> words. Between the two of us, "style points" were awarded for archaic, aplomb, axioms, eclipse, imbues, poseur, pundit, urethra.

<h2><tt>perquackey.matthewtodd.org</tt></h2>

Finally, when the time runs out, you always wonder what you missed, so I've written a <a href="http://perquackey.matthewtodd.org/">Perquackey word looker-upper</a>.

It's a little slow, but I'm working on it. (On a geek note, the best change so far has been <a href="http://matthewtodd.org/svn/public/perquackey/lib/perquackey/string.rb">splitting a string into letters</a> with <code>unpack 'c*'</code> instead of <code>split //</code>---that way, I'm comparing letters as integers rather than strings of length 1.)
