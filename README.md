
```c++
  /$$$$$$$$ /$$    /$$ /$$$$$$$$ /$$   /$$ /$$$$$$$$  /$$$$$$ 
 | $$_____/| $$   | $$| $$_____/| $$$ | $$|__  $$__/ /$$__  $$ 
 | $$      | $$   | $$| $$      | $$$$| $$   | $$   | $$  \__/ 
 | $$$$$   |  $$ / $$/| $$$$$   | $$ $$ $$   | $$   |  $$$$$$  
 | $$__/    \  $$ $$/ | $$__/   | $$  $$$$   | $$    \____  $$
 | $$        \  $$$/  | $$      | $$\  $$$   | $$    /$$  \ $$
 | $$$$$$$$   \  $/   | $$$$$$$$| $$ \  $$   | $$   |  $$$$$$/
 |________/    \_/    |________/|__/  \__/   |__/    \______/ 
```

<table>
    <tr>
        <td>
            master branch
        </td>
        <td>
            Linux/OSX <a href="https://travis-ci.com/rmxbalanque/csys"><img src="https://travis-ci.com/rmxbalanque/csys.svg?branch=master"></a>
        </td>
        <td> 
            Windows <a href="https://ci.appveyor.com/project/rmxbalanque/csys"><img src="https://ci.appveyor.com/api/projects/status/p5e3c6rdysatd6v9/branch/master?svg=true"></a>
        </td>
    </tr>
</table>

[![MIT license](https://img.shields.io/badge/License-MIT-green.svg)](https://lbesson.mit-license.org/)
[![Language](https://img.shields.io/badge/language-C++-blue.svg)](https://isocpp.org/)
[![Standard](https://img.shields.io/badge/c%2B%2B-17-blue.svg)](https://en.wikipedia.org/wiki/C%2B%2B17)

## Description:
This repository contains a templated c++17 (and up) Event system that allows for clients to attach callbacks to a host 
which then can be invoked at any time. Could also be understood as a 'act, react' pattern.
--------

## Why you should use events:
Real-time applications typically have many things that must be interconnected. Whether its a controller input resulting in the 
player jumping, audio player etc., or a time sensitive event such as a client entering a queue, this api makes it quick 
and easy to modulate those responses. This project started as a key element to a custom game engine, written in c++.
All player and NPC actions along with the startup and shutting down of systems were driven by these events, making them
crucial for a fast development.

--------

## Supported Platforms:
- The following platforms are supported:
    - Windows: Tested on Windows 10 Professional 64-bit.
    - Linux: Tested on Ubuntu 18.04.4 LTS

--------

## Requirements:
- C++17 capable compiler:
    - Visual C++ 15 (Visual Studio 2017)
    - Clang 5+
    - GCC 7+
--------

## Documentation:
Current documentation available on the [Wiki](https://github.com/BeOurQuest/Events/wiki).

--------

## FAQ:
##### How much overhead is from invoking an Event? <br>
Not much, it is the overhead of calling std::function times the number of hooked functions. <br>
(if your application is slow, it is most likely not the events)

##### How do I deal with dll and so static memory ownership? <br>
Create a wrapper function for the event. <br><br> Macro example:
```c++
    //! Creates a function wrapper for an event  
    #define STATIC_EVENT_WRAPPER(TYPE, NAME) static TYPE& Get##NAME() { static TYPE NAME; return NAME; }
    
    // Use example
    STATIC_EVENT_WRAPPER(Event<void()>, ApplicationStart)
    
    int main()
    {
      // ...
      GetApplicationStart().Invoke();
      // ...
      return 0;
    }
```

##### When I invoke my functions they don't get called in the order I hooked them? <br>
You must set the "KEEP_ORDER" template variable to true. <br> <br> Keeping order example: 
```c++ 
// When invoked, functions will be called in same order as they were hooked
Event<void(void), true> event;
```
--------