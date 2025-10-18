---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 35"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step35.html
---

Here's how the ```Add*``` methods ended up:

```
    void AddFirstRoll            (int pins) { AddRoll(pins); }
    void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
    void AddSpare                (int pins) { AddRoll(pins); ++frame; bonusRolls = Bonus::OneBonusRoll; }
    void AddStrike               (        ) { AddRoll( 10 ); ++frame; bonusRolls = bonusRolls == Bonus::NoBonusRolls ? Bonus::TwoBonusRolls : Bonus::TwoStrikesBonusRolls; }
```

It's reasonably simple and clear what's going on. That last bit in the ```AddStrike``` method isn't as simple as I'd hoped, but let's keep going for now.

I think we're almost done.  We just need to handle the bonus rolls in the last frame(s). 
Let's do the perfect game test.  The test code for a perfect game is just this:
```
    TEST_METHOD(PerfectGame)
    {
        RollMany(12, 10);
        Assert::AreEqual(300, game.Score());
    }
```
And the ```Assert``` fails by throwing an exception.

Go ahead and get this last test to pass, too and then click [next](Step36.html).
