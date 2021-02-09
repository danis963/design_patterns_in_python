The SOLID Design principles
=======

Single responsibility principle (SRP)  - Separations of concerns (SOC)
-----------

Let's suppose that have a class called journal with the following structure:
```
class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)


j = Journal()
j.add_entry("I cried today.")
j.add_entry("I ate a bug.")
print(f"Journal entries:\n{j}\n")
```

Now, we are going to break the SRP by adding more methods to the previous class:
```
def save(self, filename):
    file = open(filename, "w")
    file.write(str(self))
    file.close()

def load(self, filename):
    pass

def load_from_web(self, uri):
    pass
```

So, what is the problem with the new class?
```
class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)

    # break SRP
    def save(self, filename):
        file = open(filename, "w")
        file.write(str(self))
        file.close()

    def load(self, filename):
        pass

    def load_from_web(self, uri):
        pass

```

As we can see, with the new methods we have added a secondary responsibility to the journal, the new class not only does the journal things.
If we have some other classes like Journal and all of those types have their own save, load and load from web and we need to do some changes in the save method, for example, we need to modify all of them. So this funcionality might be have to be centrally changed at some point.
So, the best idea to solve it is having two classes:
```
class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)

    # break SRP
    def save(self, filename):
        file = open(filename, "w")
        file.write(str(self))
        file.close()

    def load(self, filename):
        pass

    def load_from_web(self, uri):
        pass


class PersistenceManager:
    def save_to_file(journal, filename):
        file = open(filename, "w")
        file.write(str(journal))
        file.close()


j = Journal()
j.add_entry("I cried today.")
j.add_entry("I ate a bug.")
print(f"Journal entries:\n{j}\n")

p = PersistenceManager()
file = ./rundir/journal.txt'
p.save_to_file(j, file)
```

So, what is the take away of this lesson is that we do not want to overload the objects with too many responsibilities.
-Anti-Pattern: is to have a god object. It is a object with a lot of responsibilities.
-Pattern: is to have smaller classes with less responsibilities.
