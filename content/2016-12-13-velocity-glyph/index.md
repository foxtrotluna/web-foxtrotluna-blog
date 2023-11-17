+++

title = "Velocity Glyph - 5 years and A Ludum Dare 37 post mortem"
date = 2016-12-13

[taxonomies]
tags = ["vault", "retrospective", "games", "gamedev"]


[extra]
enable_comments = true

+++
{% callout(type="quizzical", side="left") %}
This post is one from my vault of old posts. It does not reflect who I am now, and I'll interject with Foxtrot with some thoughts on it as a
retrospective. The quality of the main post is not up to my current standards, although I hope my interjections will help to keep you interested!
{% end %}

# Velocity Glyph - 5 years and A Ludum Dare 37 post mortem

This weekend (and Monday), I participated in Ludum Dare 37.

5 years ago, I also participated in Ludum Dare, for the first time. A lot has changed in those 5 years, as it would. 5 years is a long time. I went from making games as a hobby, to actually managing a games studio, as well as working on them as a hobby. I get paid every day to make games and that’s incredible. For many people, it’s a dream. It was damn hard work getting here, and it’s even more difficult now than ever, of course I’m better prepared for that now. I’m happy with where I am, I’m lucky to be here, and I’m really excited for what the next 5 years brings.

This time, I created an arcade racer, kinda Wipeout/Re-Volt inspired, a miniature hover racing game. I’m pretty impressed with the result. 

<!-- more -->
{{ figure(src="1-header.jpg",
          style="width: 75%;",
          position="center",
          caption_position="left",
          alt="A screenshot of a twisty dark scifi track for a wipeout-style racing game",
          caption_style="font-weight: bold; font-style: italic;") }}
{% callout(type="alert", side="right") %}
So many of my early projects are way, way too dark. I will find the light eventually, I promise!
{% end %}
          
You can download and play it for free [here](https://foxtrotluna.itch.io/velocity-glyph)

It’s funny to see how far I’ve come from my first LD, a pretty terrible platformer with awful graphics and controls. Performance was probably worse than Velocity Glyph too.

I created a few interesting systems for the game, and here’s some details on how they work.

## Hovering:
{{ figure(src="2-hovering.jpg",
          style="width: 75%;",
          position="center",
          caption_position="left",
          alt="Screenshot of the underside of a hovercraft, with debug lines showing the plane of 'gravity'",
          caption="Showing the underside of a hovercraft",
          caption_style="font-weight: bold; font-style: italic;") }}

There are two components to hovering. Firstly, ‘sticking’;

Physical gravity is actually turned off for the project, each Racer gets its own version of local ‘gravity’, and every FixedUpdate, applies a downwards force to itself, ‘down’ being whatever it decides. The racer is constantly casting a ray from each of its hover pads in the direction they’re facing. It averages down the normal of each ray hit and uses that as its definition of what down is.

I had a big problem at the start of being able to trick it into thinking ‘down’ was on the walls, if you rubbed against them hard enough, so I made gravity firstly Lerp between its current and desired definition of down, and secondly I don’t allow any drastic changes in gravity, as the tracks tend to curve smoothly anyway. It doesn’t work perfectly, but it’s a lot better than it was.

Secondly, the racers do actually hover above the ground, there’s no weird collision box magic going on, as well as applying a gravity force, they’re also applying an inverse force, allowing for a little over/undershooting of their desired distance, creating that distinctive bobbing effect. These values are slightly different for each kind of racer to make them feel a little different. The same function that checks gravity also feeds the respawn system. If it can’t find any ground for 5 seconds, it respawns you at the last checkpoint. This was because the AI occasionally gets confused and thinks it can take shortcuts via the side of the track, and promptly falls to its death.

## AI
{{ figure(src="3-ai.jpg",
          style="width: 75%;",
          position="center",
          caption_position="left",
          alt="A debug Unity window, showing visible raycast lines coming from dark spots where the hovercraft are.",
          caption="Finding the best route with Raycasts",
          caption_style="font-weight: bold; font-style: italic;") }}

The AI is pretty basic, but it does the job for the most part. I’m actually impressed with how well it works given the simplicity of it.

Basically, as you can see on the above screenshot, the AI casts a few rays, if one hits, it figures out a value between 0.2 and 1 for how far away it is, based on a curve, rather than a linear value, that way it turns harder a bit sooner than if it was right next to the wall.

If it detects walls in both directions, it figures out which way it should turn, and turns a reduced amount in that direction.

It tries to point vaguely in the direction of the next waypoint, but it takes a low priority, as the track curves around a bit, so pointing directly to it isn’t a great idea.

If it detects it’s facing a wall directly, it will slam in reverse to try to get away from the wall. It doesn’t always work, but it does solve a few issues. The AI makes it round the course 99% of the time now, in a competitive time. I have trouble winning against the AI sometimes, which is impressive for just 2 hours coding on the AI. There is no rubber banding or different values for the AI, it and the playercontroller send exactly the same info to the racer controller.

## Game Feel
{{ figure(src="4-game-feel.jpg",
          style="width: 75%;",
          position="center",
          caption_position="left",
          alt="A hovercraft in motion, with a blurry distant background and high field of view. The image feels 'fast'",
          caption="Speeding along",
          caption_style="font-weight: bold; font-style: italic;") }}
          
One of my goals right from the start was to make each racer feel different, while keeping a balance there. I think I’ve achieved that, and I’m quite happy with the result.

Each racer has slightly different thrust values, but nothing game breaking, the main key is the engine location. It actually applies thrust where the engines are, which makes the London, with its one powerful engine, feel a big more ‘slidy’ than say, the Moscow, with its 4 ‘push forward’ engines, that make acceleration feel a bit more punchy. 

Turning doesn’t just happen on a dime either, it lerps to the turn value, by a different amount, and each racer corrects itself slightly different.

Racers also have unique hover force values, so some feel bouncier than others.

Finally, particles and Audio play a big part in game feel. I had some sounds lying around on a hard drive that I modified and cut together to create unique sounds for the London and Washington. Sadly I ran out of time so the Moscow uses the same sounds as the Washington.

There are engine startup noises, idle noises and ‘rev’ noises. The rev noises fade in and also increase in pitch as you get faster, making it feel like you’re really pushing the engines. There’s a slight delay on the noises too, which I found just felt a bit better, though I can’t explain why.

## Conclusion
{{ figure(src="5-conclusion.jpg",
          style="width: 100%;",
          position="center",
          caption_position="left",
          alt="2 hovercraft in the process of overtaking, the non-player racer is named 'Dan Tsukusa'",
          caption="Usernames were given by my friends for use for the AI racers",
          caption_style="font-weight: bold; font-style: italic;")  }}

There’s so much I could say about this game that I created in just 72 hours, but I’ll leave it here. I had a lot of fun creating it, and I’d love to take it further in the future. I’d like to give a big shoutout to [Toastedkp](http://twitter.com/toastedkp) who nearly killed himself getting these incredible models to me. I look forward to seeing everyone else's entries over the next few weeks of voting!