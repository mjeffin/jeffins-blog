---
title: "Python to Go journey -part 1"
date: 2021-10-05T07:44:59+05:30
draft: false
tags: ["golang","python","musings"]
---

[Python](https://www.python.org/) is the easiest language to learn and the second-best language for every thing else.
It's an interpreted, dynamically typed and almost reads the pseudocode. It's one widely used by a variety of people for 
lots of different purposes. 

I was one of those guys who learned python so that I could use it to login to cisco routers, collect and analyse logs 
automatically without my intervention every time. [Bertrant Russel](https://harpers.org/archive/1932/10/in-praise-of-idleness/) sure 
had an influence on me. Thus stared the long and fruitful journey with python and django.

Then,in one of the projects there was requirement to consume messages from kafka and then do some processing. 
It was okay when I was consuming real time, but when it came to consuming millions of previous records from an earlier point in time,
python's speed limitations started becoming obvious. Threading or multiprocessing was an option, but it seemed like a
good opportunity to try out golang and it's famous go-routines. I have been wanting to learn Go for a long time
and when you have a real life use case, learning becomes much more interesting.

So thus I got Go-ing, one step a time. First stop was the excellent [Tour of Go](https://tour.golang.org/welcome/1), in the Go 
playground. It is a really great place to start. You just need a browser, time, little patience 
and lots of curiosity. Whenever some ad hoc scripting requirements came up in work, 
I used to do it in python first, deliver it in promised time and then re-write it in Go for fun. What took  one hour in python,
used to take one full day in Go in the beginning. But those extra efforts were totally worth it. It took some time to build up the muscle memory, but
now I can code in both the languages in equal amount of time and go is my go-to language for most things nowð

That's it about my journey. For those of you are contemplating that trip, here is some comparison between the two. The views are 
totally personal, and I might not be 100% right too. So take these with a pinch of salt

1. **Type Safety** : Go's strict typing has enabled me to catch so many bugs in the IDE itself, even before compiling. It's so much easier to write
safer, better quality code in Go because of that. You are generally more confident about your code and will have better peace of mind. You will never have to wake up  
in the middle of the night trying to fix a production issue, only to find that you were appending to a dict. What started out a list was changed into dict at a different point in time
by someone and they forgot to change it in a different place. Change like that won't even compile in Go and your IDE will show you an error right as you type. 
Programming as a whole became a better experience after moving to static typing  +1 to Go
2. **Deployments** : It's super easily to build and distribute binaries for the required platform with Go. Clearly a winner over python which is notorious for it's 
    packaging and dependency management. +1 to Go
3. **Ecosystem** : Python(30 years) is much older than Go(~10 years) and is used by much larger set of people from diverse domains. Python has a richer 
   ecosystem of libraries and communities, probably because of that. +1 to Python
4. **Versatality** : Python is used across different domains raging from web (django,flask) to iot(hassio,micropython) to data/science(numpy,pandas) to being almost all of 
   ML to devops(ansible) to GUI applications to connecting and getting logs from routers etc. It's the second-best language for everything and the best language for rapid prototyping, which according to 
   [Alex Krupp](https://news.ycombinator.com/item?id=27605052) is what every startup should start with. Go, on the hand tend to be used more in systems/cloud side of
   things where speed really matters. It takes relatively more time to develop something in Go, compared to python because it's more low level, but once it's built, it 
   would run faster and safer. It's also often that developers start with python and then migrate to go. +1 to Python
5. **Speed** : Go beats python hands down with it's lightweight [goroutines and channels](https://golang.org/doc/effective_go#concurrency). It's also pretty to fast to build go binaries. 
6. **Prototyping Speed**: Python is the best language to quickly try out a concept and put together a quick demo. 
7. **Verbosity**: Go is more low level and stricter than python and there is lesser magic. Hence, more lines of code have to be written in go for same solution than in python.
   Introduction of [generics](https://go.dev/blog/why-generics) will help to improve the situation.~~ (This is one topic in Go that I still haven't got my head around toð)~~ 
[Why Generics](https://go.dev/blog/why-generics) does a good job explaining the need for it and
[the proposal](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md) has already been accepted will be part of next release
8. **REPL/Tooling**: Python has it;s command line interface and [jupyter](https://jupyter.org/) notebooks for trying out something quick and dirty. Go has it's [playground](https://github.com/go-playground), but 
  it's online. Python package management is not perfect, but python has better tooling at the moment.

That's it for comparison. Both are great languages and I would encourage every python developer to try out Go, alteast as an intellectual exercise.

Will try to write some more detailed post translating the concepts in python to Go. 

