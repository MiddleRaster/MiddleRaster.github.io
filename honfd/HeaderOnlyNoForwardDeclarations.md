---
layout: page_with_comments
title: "Header-Only, No Forward Declarations"
permalink: /honfd/HeaderOnlyNoForwardDeclarations.html
---

Once upon a time, back when I was an IC at *StupendousCorp*, I was reading a book recommended to me by Corey Ladas, one of the co-co-co-inventors of Kanban.
It was called "Axiomatic Design: Advances and Applications" by Nam Pyo Suh. It's by a mechanical engineering professor at MIT, and since one of my degrees in in chemical engineering,
I didn't have any difficulty understanding the book:  it's all about coupling in the functional requirements and design parameters. For example, think of a faucet with a single spout and two knobs.
Suppose you have the water coming out at just the right temperature, but you want to increase the flow rate. If you turn up the cold spigot only, the flow rate increases but the temperature drops; turning the hot spigot does similarly but the temperature increases.
You would have to turn both knobs simultaneously at just the right rates (plural!) to keep the temperature the same while increasing flow rate. So something is coupled in there. On the other hand, my kitchen faucet has a lever that I push up to increase the flow rate,
but turn right or left to change the temperature:  not coupled.

That was all great, but I had real difficulties trying to apply Suh's Axiomatic Design to software. I still do, even after Corey's team members explained it to me. But it got me thinking heavily about coupling.
In particular, cyclic coupling (there are other kinds of coupling, notably temporal couplling, but that's a topic for another day). Here are some definitions:
1. **uncoupled** code:  class A knows nothing about class B, and class B knows nothing about class A.
2. **decoupled** code:  class A uses class B in some way (data-member, argument, base, etc.), but class B knows nothing about A.
3. **fully coupled** code:  class A knows about class B and B knows about A.  Or worse, A -> B -> C -> ... -> X -> Y -> Z -> A:  a really big cycle.

Now #1 is great, but you can't write much software that way, I don't think. #3 is really bad:  it's why a seemingly innocuous little change over here breaks something way other there. #2 is the best we can do.

Now, you can get fully coupled code only if there is a forward declaration somewhere (to make it compile). So, I thought to myself, "I wonder what would happen if I disallowed that in my code." 
I was working on v2 or 3 of my [TDD harness](https://github.com/MiddleRaster/tdd4cpp) at the time, and following my new rules forced me to clean up my design a bit, which was quite interesting to me.

Here are the rules:
 - put all the code in headers, all the methods fully defined inside the class, between the opening ```{``` and the closing ```};```.
 - never, ever (well, hardly ever) use a forward declaration.

If you follow both of these rules any attempt however inadvertent to put in a cycle will result in a compiler error. And that's huge. Have you ever seen a dependency diagram of a large-ish codebase?  It's almost invariably a huge ball of spaghetti.
But if you follow my two rules, the code must by definition be completely dendritic, all DAGs, no cycles at all.

So, I started doing this on all my work. Fixing bugs became much easier, no unexpected failures over there when making changes over here, and the code was simpler to understand. All good things, so when I became a lead at *StupendousCorp*, I forced all my reports
to do the same. Interestingly, every single one of them came to me and said, "Hey, you know that crazy all-code-in-headers idea of yours? It doesn't work." But when we went over their code together, I would invariably find that they were trying to put in a dependency
cycle, but it was caught by the compiler.

So, does this work? (Yes.) Can you write all code like this? (No.) Will it take longer or will it be faster? It turns out to be much faster. Here's a story from my experience in those early days. I was working in an area where I didn't have much expertise,
but my manager did. We were doing 1-week iterations (my favorite length), and at a demo meeting, she asked, "How are you going to handle blah?" I didn't get it, so I ignored the problem. At the next week's demo meeting, she said the same thing, so I started
thinking about it, but the lights didn't go on until the next morning in the shower (I know, too much information). On Monday, I came in and said to my guys, "I get it; I know what she wants" and the architectural change required was a "large", what we called a "3".
A "3" was 3 weeks for us back then (I hadn't figured out #NoEstimates yet). I took on the work myself and got on it. And I was done in 2.5 *days*. I spend a whole 'nother day just staring at the code, wondering why this large change was done so quickly.
That's more than a 5x improvement. I attribute it to following the two rules because it made the code dendritic and simple to understand and modify.

C++ is the only language I know that has "declare before use" semantics (there are probably others, but I don't know them). For other languages, like C#, you'd have to write a tool, using reflection, to run through the code looking for cycles. So I did this, too,
and ran it against all the code in my division that I could get my hands on.  At the time at *StupendousCorp* there were two teams writing essentially the same code with identical iterfaces, but for different platforms. One team was doing TDD, the other did not.
When I ran my tool against the TDDing team, I found 10 cycles; when I ran it against the non-TDDing team, I found 200 cycles. That's more than an order of magnitude difference. Conclusion: doing TDD discourages people from putting in dependency cycles
and that makes sense: it would feel very odd to have to create a whole slew of objects, just to unit test one. Guess which team had all the bugs? Bugs come about through carelessness or more often misunderstanding your own code.
My rules make the code way easier to understand and that's a very Good Thing (tm).

#### Objections / FAQs

We're down to the "Frequently Asked Questions" part of our show, also known as "objections".

Q. You've just reinvented Java (or C#).

A. This is what you get when you only pay attention to the first rule, but not the second. You need to follow them both or it won't work. And, no, Java doesn't have this feature. Neither does C#.
<br><br><br>
Q. B-b-but John Lakos in his book, "Large-Scale C++ Software Design" says never to include a header when a forward declaration will do.

A. This is true. Herb Sutter and Andrei Alexandrescu say the same thing in their book, "C++ Coding Standards: 101 Rules, Guidelines, and Best Practices". They're wrong. Lakos's advice was correct at the time, because back when he wrote it (1996), computers and
compilers were much slower than they are today; nowadays compilers are really good at templates  and they're header-only. Lakos has changed his tune and instead now says, "Strive for header-only code".
<br><br><br>
Q. I like seeing my prototypes and interfaces in one file and the implementations in another.

A. This is only because that's what you're used to. C# and Java people are used to not having separate files.
In fact, I would argue that if you can't see all your code at a glance in one screenful, your classes are too big.
Most people react badly when they first hear my rules. One exception was Bob Jervis of Turbo C fame. He was at Google
when I told him of my idea and he immediately said, "You are making good use of a quirk of ANSI C to enforce very
fine layering." Exactly right.
<br><br><br>
Q. Won't rule number 1 implicitly inline every method?

A. Yes. The compiler is really good at this and can make better decisions at compile- or link-time than humans can. Think of boost or STL or other large header-only libraries. We use them without a second thought.
<br><br><br>
Q. Above you said, "No" to "Can your write all code like this?" When can you not?

A. There are times where A must know about B and B must know about A. An example is edges and vertices in graphing. When you have a large cycle, get your architects involved. Try to shrink it down to as small as possible. 
Note that my intent is to protect against introducing **accidental** cycles. If you *have* to design one in, think hard about not doing so; talk to more experienced devs. But if it's unavoidable, then do it but try to put it all in one class or file, if possible.
<br><br><br>
Q. I'm shipping a .dll. If I put all code in my headers, I'll have to ship them.

A. This is the "(well, hardly ever)" part, above. In this case, you pull all your header code into a single .cpp file, with a DllMain function, as well as any exported functions, and wrap the calls to your headers using the PImpl idiom.
<br><br><br>
Q. Do you never use .cpp files?

A. For an executable, I have a single .cpp file with ```main``` or ```WinMain``` that #includes the headers I use.  Ditto for a .dll. However, since I TDD all my code, I have lots of unit tests and they all live in .cpp files. Build times are usually blazingly fast,
since I usually only have to build a single .cpp file, the one that corresponds to the change I'm making in the header. Very occasionally, I'll change a leaf-most node and many .cpp file will have to be rebuilt, but it happens quite rarely,
as it's like changing your string class:  hopefully not something you do very often.
<br>

#### Conclusion
Go ahead and give it a try, maybe on a small project first. You'll see cleaner architecture almost immediately, but probably won't see how much easier the code is to understand until you have about 50k lines of code. But by all means, give it a try.
