# Function A Day Keeps the Brain Lag Away

## What is this site?

This is the blog companion of [this YouTube Series.](linkherelol). Sub and click the bell to get all the notifications

Epidsodes are listed from newest to oldest.

- [Episode 1](### Episode 1 - isempty, and why this series needs to exist)



### Episode 1 - isempty, and why this series needs to exist

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

