---
layout: post
status: publish
title: 'W.O.M.M. Project Euler #3'
slug: "W-O-M-M-Project-Euler-3"
---
![works-on-my-machine-starburst][1] 

Here we are with another excellent installment in the "Works On My Machine" series where I post some code, some thoughts and hopefully show you something interesting/cool/new.

Today I'm going to talk briefly about Project Euler problem #3. Problem 3 asks you to find the largest [prime factor][2]  of the number 600,851,475,143.

My solution is pretty simple but it works:
    
    var getLargestPrimeFactor = function(num)
    {
        var factors = [];
        for ( var f = 2; f &lt; num; f++ )
        {
            if( num%f == 0 )
            {
                factors.push(f);
                num = num /f;
            }
        }
        factors.push(num);
        return factors[factors.length-1];
    };
    
    alert( getLargestPrimeFactor(600851475143));
    

[Try this code!][3] 

You may have noticed that there isn't a check for a factors optimus primeness anywhere in there.

Thats because the `getLargestPrimeFactor()` function is dividing the number by it's smallest factor, and the smallest factor for any number is always prime so when we run out of factors we've got the largest prime factor. Yeah, that's pretth cool IMO.

I read the discussion threads for problem 3 after I solved it and I was surprised that hardly anyone removed the prime checks from their code.

So, problem 3 is in the bag and I feel pretty good about coming up with the prime check optimization (or lack thereof). A small bit of proof that I know what I'm doing. Sometimes :D
  [1]: http://codeimpossible.com/wp-content/uploads/2009/06/works-on-my-machine-starburst.jpg
  [2]: http://en.wikipedia.org/wiki/Prime_factor
  [3]: http://beta.jsvudo.com/76cc38
