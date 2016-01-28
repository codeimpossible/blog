---
layout: post
status: publish
title: 'W.O.M.M. #5 Project Euler - Problem 2'
slug: "W-O-M-M-5-Project-Euler-Problem-2"
---
![works-on-my-machine-starburst][1] 

[Project Euler][2]  is an awesome website that I found out about a while back via [this question on StackOverflow][3] . Basically, it's a website that poses a ton of programmer related challenges and it keeps track of the problems that you solve.

The problems start out relatively easy (Add all the natural numbers below one thousand that are multiples of 3 or 5.) and progress all the way to insane (Caradano Triplets and Covex Holes).

I'm on the easy side, I just started working through them tonight and thought it would be cool to post how I build the solution to each problem as I complete them.

[Problem 2][4]  is pretty easy:

"Find the sum of all the even-valued terms in the Fibonacci sequence which do not exceed four million."

So first things first, I'll need to pick a language to use to solve this. I chose javascript simply because it was something I knew I could get running pretty quickly.

Next I'll need some variables. (note: if you're not familiar with Fibonacci numbers [Wikipedia has an awesome article on them][5] .)

    
    var start = 1, end = 4000000, current = 1, previous = 1, sum = 0, temp = 0;
   
That should do. I've got my start variable (dropping the 0 in favor of more readable code), my end counter, and my "where-am-i" variables: current, previous, temp and my sum variable.

Obviously we'll need some kind of loop. How about a while loop?
    
    while( current &lt;= end )
    {
    
    }

Cool. Now when the loop is running I'll need a way to keep track of the current value, the previous value and I need to add the current value to the sum. So....
    
    while( current &lt;= end )
    {
        temp = current;
        current = current + previous;
        sum += current;
        previous = temp;
    }

And there we hav... Oh Wait!

I overlooked a pretty important part of the problem description: "sum of all the even-valued terms"!!!

So given this new(ish) piece of info my solution end sup being:
    
    var start = 1, end = 4000000, current = 1, previous = 1, sum = 0, temp = 0;
    
    while( current &lt;= end )
    {
    temp = current;
    current = current + previous;
    
    if( current%2 == 0 )
    sum += current;
    
    previous = temp;
    }
    
    alert(sum);

  [1]: http://wpup.codeimpossible.com/2009/06/works-on-my-machine-starburst.jpg
  [2]: http://projecteuler.net/
  [3]: http://stackoverflow.com/questions/4002/code-katas
  [4]: http://projecteuler.net/index.php?section=problems&amp;id=2
  [5]: http://en.wikipedia.org/wiki/Fibonacci_number
