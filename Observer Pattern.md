### Observer Pattern

Based on the publish/subscribe model of programming.

The purpose is to decrease coupling and allows abstraction away from the observer and the subject while allowing communication.

This is a "1 to many" dependency.

!includeurl https://raw.githubusercontent.com/linux-china/plantuml-gist/master/src/main/uml/plantuml_gist.puml
![PlantUML model](http://www.plantuml.com/plantuml/png/NP0npi8m38NtdCBZ_m-z068e4X8Z0oTmYu6Yj46sMwc2tXrZce8OyxsyxqKfHP6rRsFGT2Qz4CCz060bobWhr155uD1NLwhL8u1K2V50B2iOZ2PssYLjJkCcnhz_Eq_EtkqTjkHLvrFudlePshlQasmqdLMoQzl8BUBuqRt235DH-5YOt-iWAyFYNZyCpvtbgaClbzGTxKEU)

@startuml
together {
  abstract class Subject {
    observers: vector
    notifyAll()
  }
    class Game {
  
  }
}

together {
abstract class Observer {
  notify()
}

  class Display {
  
  }

}

Display o-- Game
Subject o-- Observer
Observer <|-- Display
Subject <|-- Game
@enduml

<p>
<p>
The idea behind this is to allow the Game to notify the display without needing to know how display is implemented.

- We give it a single access point called notifyAll() that calls notify() on all of the observers.

- The display may need information about the game, so it carries a pointer in case it needs access (preferably limiting the amount of info that is provided).

#### Observer.h
```c++

#ifndef _OBSERVER_H_
#define _OBSERVER_H_

class Observer {
 public:
  virtual void notify() = 0;
  virtual ~Observer();
};

#endif
```

#### Observer.cc
```c++
#include "observer.h"

Observer::~Observer() {}
```

#### Subject.h
```c++
#ifndef _SUBJECT_H_
#define _SUBJECT_H_
#include <vector>
#include "observer.h"

class Subject {
  std::vector<Observer*> observers;

 public:
  Subject();
  void attach(Observer *o);
  void detach(Observer *o);
  void notifyObservers();
  virtual ~Subject()=0;
};

#endif
```

#### Subject.cc
```c++
#include "subject.h"

Subject::Subject() {}
Subject::~Subject() {}

void Subject::attach(Observer *o) {
  observers.emplace_back(o);
}

void Subject::detach(Observer *o) {
  for (auto it = observers.begin(); it != observers.end(); ++it) {
    if (*it == o) {
      observers.erase(it);
      break;
    }
  }
}

void Subject::notifyObservers() {
  for (auto ob : observers) ob->notify();
}

```
