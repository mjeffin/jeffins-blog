---
title: "Python to Go journey -part 1"
date: 2021-10-05T07:44:59+05:30
draft: false
tags: ["golang","challenge"]
---

### Introduction

[Python](https://www.python.org/) is the easiest language to learn and the second-best language for every thing else.
It's an interpreted, dynamically typed and almost reads the pseudocode. It's one widely used by a variety of people for 
lots of different purposes. 

I was one of those guys who learned python so that I could use it to login to cisco routers, collect and analyse logs 
automatically without my intervention every time. [Betrant Russel](https://harpers.org/archive/1932/10/in-praise-of-idleness/) sure 
had an influence on me. Thus stared the long and fruitful journey with python and django.

Then in one of the projects, there was requirement to consume messages from kafka and then do some processing. 
It was okay when I was consuming real time, but when it came to consuming millions of previous records from an earlier point in time,
python's speed limitations started becoming obvious. Threading or multiprocessing was an option, but it seemed like a
good opportunity to try out go and it's famous go-routines. I have been wanting to learn go for a long time. When you have a real life use case, learning becomes much more interesting.

So thus I started learning Go, one step a time. Started with the excellent [Tour of Go](https://tour.golang.org/welcome/1), in the go 
playground. It is a really great place to start. You just need a browser, time, little patience and lots of curiosity. Once I became 
slightly familiar, I started implementing it in my work. Whenever some ad hoc scripting requirements came up, I used to do it in python, 
deliver it and then re-write it in Go. What took me one hour in python, used to take one full day in Go. But those extra efforts were 
totally worth it. It took some time to build up the muscle memory and now I can code in both the languages in equal amount of time and 
go is my go-to language for most things nowð

That's it about my journey. For those of you are contemplating that trip, here is some comparison between the two. The views are 
totally personal, and I might not be 100% right too. So take these with a pinch of salt

1. **Type Safety** : Go's strict typing has enabled me to catch so many bugs in the IDE itself, even before compiling. It's so much easier to write
safer, better quality code in Go because of that. You are generally more confident about your code about ... +1 to Go
2. **Deployments** : It's super easily to build and distribute binaries for the required platform with Go. Clearly a winner over python which is notorious for it's 
    packaging and dependency management. +1 to Go
3. **Ecosystem** : Python(30 years) is much more older than Go(~10 years) and is used by much larger set of people from diverse domains. Python has a richer 
   ecosystem of libraries and communities, probably because of that. +1 to Python
4. **Versatality** : Python is used across different domains raging from web (django,flask) to iot(hassio,micropython) to data/science(numpy,pandas) to being almost all of 
   ML to devops(ansible) to GUI applications to connecting and getting logs from routers etc. It's the second-best language for everything and the best language for rapid prototyping, which according to 
   [Alex Krupp](https://news.ycombinator.com/item?id=27605052) is what every startup should start with. Go, on the hand tend to be used more in systems/cloud side of
   things where speed really matters. It takes relatively more time to develop something in Go, compared to python because it's more low level, but once it's built, it 
   would run faster and safer. It's also often that developers start with python and then migrate to go. +1 to Python
5. **Speed** : Go beats python hands down with it's lightweight [goroutines and channels](https://golang.org/doc/effective_go#concurrency). It's also pretty to fast to build go binaries. 
6. **Verbosity**: Go is more low level and stricter than python and there is lesser magic. Hence more lines of code have to be written in go for same solution than in python.
   It's said that the introduction of [generics](https://go.dev/blog/why-generics) will help to improve the situation. (This is one topic in Go that I still haven't got my head around toð)
7. **REPL/Tooling**: Python has it;s command line interface and [jupyter](https://jupyter.org/) notebooks for trying out something quick and dirty. Go has it's [playground](https://github.com/go-playground), but 
  it's online. Python package management is not perfect, but python has better tooling at the moment.

That's it for comparison. Both are great languages and I would encourage every python developer to try out Go, alteast as an intellectual exercise.

Will try to write some more detailed post translating the concepts in python to Go. 

