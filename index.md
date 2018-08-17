# Function A Day Keeps the Brain Lag Away

## What is this site?

This is the blog companion of [this YouTube Series.](linkherelol). Sub and click the bell to get all the notifications

Epidsodes are listed from oldest to newest, and archived in the reverse.

- [Episode 1 - isempty, and why this series needs to exist](###Episode-1-\--isempty,-and-why-this-series-needs-to-exist)
- [Episode 2 - A useful example of imread's transparency detection](###Episode-2-\--A-useful-example-of-imread's-transparency-detection)


### Episode 2 - A useful example of imread's transparency detection

In [Episode 1](###Episode-1-\--isempty,-and-why-this-series-needs-to-exist) I expressed my frustration that the imread function ,which is almost certainly a user's first exposure to the image processing toolbox, is so poorly documented. Today we'll fix that.

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
imwrite(A,'transpepper.png','Alpha',double(A(:,:,1)>230));%make the image completely transparent where the pepers are red and write to a new image
[X,map,alpha] = imread('transpepper.png');%read in the new image and check if there is a transparent layer
                                          %note that these are the same lines as the original documentation
isempty(alpha)

```

Well... That was easy. Let's check out what that image looks like!

```
figure(1)%good form
image(B,'AlphaData',Alpha)%our base image
figure(2)%more good form
image(Alpha)%our updated image
```

Well, that was 8 lines of code to clearly demonstrate!! Hope this little tutorial has helped. Luckily, while doing research for this episode I've come across an actual error in MATLAB's documentation, so I've got a little material left to talk about.

[`TransparentColor`](https://www.mathworks.com/help/matlab/ref/imwrite.html#btv3cny-1-TransparentColor) defines what the "transparentness" in your image should show as when you're using `imwrite`. But there's a problem. Let's try adding their example to our imwrite line above. Simply add a hint of: `'TransparentColor',20`

```
imwrite(A,'transpepper.png','Alpha',double(A(:,:,1)>230),'TransparentColor',20);
```

run it and.... we get an error???

```
Error using writepng>CheckTextItem (line 378)
Text chunk must be a character vector.

Error in writepng (line 258)
        item = CheckTextItem(unmatched.(param_name));

Error in imwrite (line 485)
        feval(fmt_s.write, data, map, filename, paramPairs{:});
```
Weird. A little googling shows what one migth expect, that we're feeding something other than a character vector where a character vector belongs. But we know our code works before we add the example so... could it be??? Let's make that last argument a character vector.

```
imwrite(A,'transpepper.png','Alpha',double(A(:,:,1)>230),'TransparentColor','20');
```






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

These will hopefully serve as useful reference points on not only the functionality of some MATLAB and Octave functions, but also how they interact and behave.

