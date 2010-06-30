--- 
wordpress_id: 19
layout: post
title: Stubbing/Mocking for ThinkingSphinx and will_paginate using Flexmock
wordpress_url: http://panpainter.com/?p=19
---
One of the things that I love about Rails development is the emphasis on Test Driven Design (and BDD, but that's another topic entirely). As someone relatively new to Rails, however, it's occasionally frustrating to attempt to build tests when you don't full know what to expect back (not knowing what to expect as a returned value, being dependent on an outside resource like an API, whatever).

Nothing has been as frustrating for me as learning about mocking/stubbing.  It's one of those things that everyone else just seems to know, but rarely talks about.  Maybe I'm just looking in the wrong places; I'm not sure.  Either way, I was recently working on adding in ThinkingSphinx to a project that I was working on, and I wanted to make sure that my implementation of the plugin was being tested (not, of course, testing ThinkingSphinx itself, because it's already tested in its own right).

Of course, anything that runs through the plugin is eventually going to want to hit the search daemon, which is a dependency that our test suite shouldn't have.  This is easy enough to overcome with a quick mock (if you know the right magic incantation, that is).

After some searching, I settled on Flexmock as the framework of choice, if for no more reason than the fact that it has better documentation than some of the others I've found.  Here's the test that I wrote that just made sure that search would occur given a few different situations. (Note that this is done using `Test/Unit`.)

First I had to make sure that will_paginate would cooperate with me, so I created a `view_helper.rb` file in `/test/mocks/test/` to set the `total_pages_for_collection` method to just return "1" (because we really don't care how many pages there are in the collection, and running anything against a view without this being set throws an error):

    # /test/mocks/view_helpers
    
    require 'will_paginate/core_ext'
    
    module WillPaginate
      module ViewHelpers
        def self.total_pages_for_collection(collection) #:nodoc:
          return 1
        end
      end
    end

The only slightly odd thing here is the `#:nodoc:` statement; we just don't want to have this method show up in any of the documentation (probably not really necessary, but I feel better having it in place). Now that will_paginate is getting pre-empted, we can test the `SearchController`:

    # /test/functional/search_controller_test.rb
    
    require 'test_helper'
    require 'flexmock/test_unit'
    require 'mocks/view_helpers'
    
    class SearchControllerTest < ActionController::TestCase
    
      test "should search events" do
    
        flexmock(Location).should_receive(:search).once.with_any_args.and_return([locations(:one)])
        flexmock(Event).should_receive(:search).once.with_any_args.and_return([events(:one)])
        
        get :index, :q => 'hello world'
        assert_response :success
    
        assert_match '<p>There is 1 result for "hello world"</p>', @response.body
    end
  
    test "get with no query paramter searches everything" do

        flexmock(Event).should_receive(:search).once.with_any_args.and_return([events(:one)])
    
        get :index
        assert_response :success
        assert_match 'There is 1 result for ""</p>', @response.body
      end
    end

I've got two tests here, one that tests that search works if a query is passed to the search daemon, and one that tests what happens if no query is actually passed (the expected function for the latter test is that all Events would be displayed).

I also like Flexmock because of the readability of the tests &ndash; the method names are quite literally what they seem to be. The really important thing to note though, and what took me forever to find, was the `.once.with_any_args.and_return(#...)` phrasing. That's what's really key here &ndash; I was getting all manner of weird errors back until I set up my stub this way. What I'm doing is short-circuiting the search function &ndash; I'm telling my tests that the search parameters and responses are unimportant, all that we really want is to make sure that the search method is getting invoked.

Hopefully this will help save someone else time. If there are better ways to solve this problem, I would love to hear about it &ndash; like I mentioned above, mocking/stubbing seems to be one of those topics that no one really discusses at any length, so the more discussion we can have, the better.

<p>Shortlink: <a href="http://panpainter.com/p/6">http://panpainter.com/p/6</a></p>
