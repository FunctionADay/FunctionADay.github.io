# Function A Day Keeps the Brain Lag Away

## What is this site?

This is the blog companion of [this YouTube Series.](linkherelol). Sub and click the bell to get all the notifications

Epidsodes are listed from oldest to newest, and archived in the reverse.

- [Episode 1 - isempty, and why this series needs to exist - 16/08/18](#episode-1)
- [Episode 2 - A useful example of imread's transparency detection - 17/08/18](#episode-2)
- [Episode 3 - A not-so-basic guide to MATLAB naming convention - 20/08/18](#episode-3)

# Episode 4
## Indexing in MATLAB and why indexing from 1 isn't the devil it is made to be









# Episode 3
## A not-so-basic guide to MATLAB naming convention

It's important in any language, but in MATLAB in particular, to make sure that you're not using a protected variable or function handles. MATLAB actually has a function ( `iskeyword` ) that returns a list if you ever forget which names you cannot use. This is great, but in these instances, MATLAB will actually stop you and tell you that you cannot use these in the way that you're trying to.

```
>> iskeyword

ans =

  20×1 cell array

    {'break'     }
    {'case'      }
    {'catch'     }
    {'classdef'  }
    {'continue'  }
    {'else'      }
    {'elseif'    }
    {'end'       }
    {'for'       }
    {'function'  }
    {'global'    }
    {'if'        }
    {'otherwise' }
    {'parfor'    }
    {'persistent'}
    {'return'    }
    {'spmd'      }
    {'switch'    }
    {'try'       }
    {'while'     }
>> break =3 
break =3 
      ↑
Error: Incorrect use of '=' operator. To assign a value to a variable, use '='. To compare values for equality, use '=='.
    
```

But obviously you don't want to write over a function, especially not our good friend `imread()`

```
>> imread = (1:2)

imread =

     1     2

>> imread('image.jpg')
Index exceeds array bounds.

'imread' appears to be both a function and a variable. If this is unintentional, use 'clear imread' to remove the variable 'imread'
from the workspace.
```
UhOh... 

Now this wouldn't really be a problem if the [styleguides for MATLAB](https://www.google.com/search?rlz=1C1NHXL_enUS801US801&ei=FCN7W-G-N8jusQWSuJXoBw&q=filtype%3Apdf+matlab+naming+convention&oq=filtype%3Apdf+matlab+naming+convention&gs_l=psy-ab.3...3168.5678.0.5919.12.12.0.0.0.0.122.753.11j1.12.0....0...1.1.64.psy-ab..0.3.205...0i13k1j0i8i7i30k1j0i13i30k1j0i8i13i30k1.0.-HCmvnoKm68) didn't almost universally suggest lowerCamelCase as the prefered naming convention for variables. The concensus is that since this is a common naming convention in other languages as well, might as well use it for MATLAB. However my experience as a TA has taught me that this should NOT be encouraged (at least among students).

If you were to teach students lowerCamelCase as your naming convention, then told them to import and read an image, you can't mark them for trying:

```
>> image = imread('peppers.png');
>> image(image)
Array indices must be positive integers or logical values.

'image' appears to be both a function and a variable. If this is unintentional, use 'clear image' to remove the variable 'image'
from the workspace.
```

And unfortunately googling for that error code is rather unhelpful. So now you're stuck explaining, "well of course you shouldn't have named it the same as a function, what were you thinking?" but that makes you seem *kinda* like a dick. Plus, even if you tell them this upfront, new users aren't going to have nearly the same level of encyclopedic knowledge of MATLAB function names to avoid. Fundamentally the problem with lowerCamelCase is that it necessitates lowercase first words and *many* of the functions new users to MATLAB will be encountering could (very reasonably) used as variable names. . So what's the solution?

UpperCamelCase! Some will say well what about things like `VideoWriter` and then I would say "well what about things like `writeVideo`!!!

My personal opinion is that if you teach UpperCamelCase to a new MATLAB user, by the time where they may encounter functions they can overwrite they will have already learned enough to debug their error.


# Episode 2
## A useful example of `imread()`'s transparency detection

In [Episode 1](#Episode-1:-isempty,-and-why-this-series-needs-to-exist) I expressed my frustration that the imread function ,which is almost certainly a user's first exposure to the image processing toolbox, is so poorly documented. Today we'll fix that.

The issue is with the example of the transparent case `[A,map,transparency] = imread(___)` shows the case where an image *doesn't* have a transparent layer. Certainly it's important to know that it returns the empty set but `if the image does not have a transparent layer, the empty set is assigned to transparency` would suffice.

So let's write an example of our own, starting with the example that the documentation gives us. We have:

```MATLAB
>> [X,map,alpha] = imread('peppers.png');
>> isempty(alpha)

ans =

  logical

   1
```

I think our goals should be 1) use peppers.png as our base image, 2) as few lines as possible

```MATLAB

A = imread('peppers.png');%load in pepper.png
imwrite(X,'transpepper.png','Alpha',double(A(:,:,1)>230));%make the image completely transparent where the pepers are red and write to a new image
[X,map,alpha] = imread('transpepper.png');%read in the new image and check if there is a transparent layer
                                          %note that these are the same lines as the original documentation
isempty(alpha)

```

Well... That was easy. Let's check out what that image looks like!

```
figure(1)%good form
image(B,'AlphaData',alpha)%our base image
figure(2)%more good form
image(alpha)%our updated image
```

Well, that was 8 lines of code to clearly demonstrate!! Hope this little tutorial has helped. 




# Episode 1
## `isempty()`, and why this series needs to exist

To watch this demonstration visit []()

First, what is `isempty()` well, just as the name implies it's the MATLAB function for checking if an array is empty. That's it. It doesn't check for what type of array it is, it just checks if there is something there and then says "ok cool, yep there's is(n't) something there"

Before I get into my rant there's actually a REAL lesson to be learned and that is [MATLAB has a list of functions like this!!](https://www.mathworks.com/help/matlab/ref/is.html) and they are called the 'detect state' functions (I guess?). Anyways, for those with some VBA the function names certainly feel similar, but ymmv.

If only every function could be this easy to understand. 

MATLAB is known as a language that is learn in large part because of its generally very useful documentation. However there are some things that just make you think "why?" One of those moments drove me to make the inagural episode on our beloved `isempty()`. MATLAB's documentation is often *precise* but lacking in *practicality*. It's understandable that you want to show the functionality for isempty by running `isempty([])` but sometimes you just want to see a little bit of context, or a function used to solve a short problem. So here's my example.

We want to check to see if an image has a transparent layer. We do that by loading the image an checking if the alpha variable (where the transparent layer would be) `isempty()`.

```Matlab
>> [X,map,alpha] = imread('peppers.png');
>> isempty(alpha)

ans =

  logical

   1
```
Sure enough it is!!

Now, back to my rant. Why on earth is the documentation for the `imread()` case where you have a transparent layer the counterexample!! The example from the MATLAB docs is copied below.

```Matlab
[X,map,alpha] = imread('peppers.png');
alpha
alpha =

     []
     
```
     
```
No alpha channel is present, so alpha is empty.
```
My only hope would be that this was just a placeholder until they start to include a transparent image so that they can show actual code example, but until then, we'll have to wait for episode two!!

These will hopefully serve as useful reference points on not only the functionality of some MATLAB and Octave functions, but also how they interact and behave.

