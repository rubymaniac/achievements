# Achievements

Achievements is a drop in Achievements/Badging engine designed to work
with Ruby, ORM backed web applications.

Achievements uses Redis for persistence and tries to be as abstract as
possible, essentially acting as a counter server with assignable
thresholds.

The application Achievements was developed for uses a User class to
instantiate the Engine and an Achievement class to persist the details
of the achievements on the application side.

## Achievements, Briefly

Here's how you get achievements into your application:

    require 'achievements'

    class Engine
      # Include the AchievementsEngine module from the Achievements
      # gem to make your Engine class an AchievementsEngine class. 
      include Achievements::AchievementsEngine
      
      # Now you can define achievements in this class.  One at a time
      # using the achievement method:

      # One achievement, one level
      achievement :context1, :one_time, 1
      
      # Two achievements, one level
      achievement :context2, :one_time, 1
      achievement :context2, :three_times, 3

      # One achievement, multiple levels
      achievement :context3, :multiple_levels, [1, 5, 10]
      
      # Or all at once using the achievements (plural) method.  Right
      # now this relies on the existence of a class which responds to
      # id, name, and achievement methods:
      achievements Achievement.all
    end

Here's how you could interact with it in a session with the above
class loaded:

    # Grab the engine 
    @engine = Achievements.engine

    # Achieve something!
    @engine.achieve(:context1, 1, :one_time)
    => [[:context1,:one_time,"1"]]    

For the most up to date look at what this library is supposed to do,
please refer to the test directory.
    
## Details

### Contexts

Contexts are categories for achievements.  Every time an achievement
is triggered, it's "parent" or context counter is also triggered and
incremented.  This makes it easier to gauge overall participation in
the system and makes "score" based calculations less expensive.

### Achievements

Achievements are named, contextualized counters with one or more thresholds. 

### Output

When a threshold isn't crossed, and nothing changes, the engine will
return nothing when triggered.

When a threshold is crossed, the engine will return an array of
symbols which correspond to the names of the achievements which have
been reached, along with the threshold being crossed.  Your application can consume this output as you see
fit.

### Achievement API Compliance

Your Achievement class must have a name, context, and threshold method
in order to adapt to the Engine, in order to be consumed by the Agent
class directly.

## Project Metainfo

### Dependencies

* A running Redis server
* The Redis Ruby Gem

### TODO

* Better Redis interface methods for making/closing/keeping conneciton

### Contributing

0. Do some work on the library, commit
1. [Fork][1] Achievements
2. Create a topic branch - `git checkout -b my_branch`
3. Push to your branch - `git push origin my_branch`
4. Create an [Issue][2] with a link to your branch
5. That's it!

### Author

Michael R. Bernstein - *michaelrbernstein@gmail.com* - twitter.com/mrb_bk

### Sponsorship

The Achievements project was 100% sponsored by [Intersect NYC][3] in an ongoing effort to give back to the Open Source community.  

[1]: http://help.github.com/forking/
[2]: http://github.com/mrb/achievements/issues
[3]: http://intersectnyc.com
