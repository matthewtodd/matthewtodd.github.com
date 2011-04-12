---
title: Using Authlogic and ActiveResource
layout: post
---

<p class="update"><strong>Update, 6 July 2009:</strong> This code is outdated, designed around Authlogic 1. Thankfully, though, Joe Scharf (<a href="http://matthewtodd.org/2009/02/19/using-authlogic-and-active_resource.html#comment-12162178">in the comments</a>) has bundled up an <a href="http://github.com/quantipay/authlogic_haapi">HTTP-basic-authentication-via-API-key add-on</a> for Authlogic 2. Well done, sir!</p>

Out of the box, Authlogic identifies the current user by looking for [four different values](http://github.com/binarylogic/authlogic/blob/7cd869f49a264cb7ece7e72df8ff077c06fdc5d3/lib/authlogic/session/config.rb#L107-120):

- The user's "single access token" in the [request parameters](http://github.com/binarylogic/authlogic/blob/7cd869f49a264cb7ece7e72df8ff077c06fdc5d3/lib/authlogic/session/params.rb).
- The user's id in the [session](http://github.com/binarylogic/authlogic/blob/7cd869f49a264cb7ece7e72df8ff077c06fdc5d3/lib/authlogic/session/session.rb).
- The user's "persistence token" in a remember me [cookie](http://github.com/binarylogic/authlogic/blob/7cd869f49a264cb7ece7e72df8ff077c06fdc5d3/lib/authlogic/session/cookies.rb).
- The user's login and password in an [HTTP basic authentication](http://github.com/binarylogic/authlogic/blob/7cd869f49a264cb7ece7e72df8ff077c06fdc5d3/lib/authlogic/session/base.rb#L341-352) header.

This is pretty awesome---most things you'd like to do Just Work.

But I'd like to make one small adjustment, since I'm using ActiveResource and following the example of the [Highrise API](http://developer.37signals.com/highrise/): rather than doing HTTP basic authentication with the user's login and password, I'd like to use an API key instead.

It turns out Authlogic makes this fairly easy:

{% highlight ruby %}
class UserSession < Authlogic::Session::Base
  # Adjust how Authlogic identifies the current user.
  # The default setting is :params, :session, :cookie, :http_auth.
  find_with :session, :cookie, :api_key
  
  def valid_api_key?
    controller.authenticate_with_http_basic do |api_key, _|
      self.unauthorized_record = search_for_record("find_by_#{single_access_token_field}", api_key)
      self.persisting = false
      self.valid?
    end.tap do |authenticated|
      self.persisting = true unless authenticated
    end
  end
end
{% endhighlight %}

So now I can write a Widget Resource class like this:

{% highlight ruby %}
class Widget < ActiveResource::Base
  self.site = 'http://localhost:3000/'
  self.user = 'MY_API_TOKEN'
end
{% endhighlight %}

Or poke around with `curl`:

{% highlight bash %}
# Create a new, empty Widget.
curl --user   'MY_API_TOKEN:X'                \
     --header 'Content-Type: application/xml' \
     --data   '<widget></widget>'             \
     http://localhost:3000/widgets.xml
{% endhighlight %}
