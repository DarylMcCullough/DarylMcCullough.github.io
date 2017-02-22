---
layout: post
title: Hanukkah Lights
thumbnail-path: "img/hanukkah-2017.jpg"
short-description: Hanukkah Lights is a javascript animation of the lighting of Hanukkah candles

---

{:.center}
![]({{ site.baseurl }}/img/hanukkah-2017.jpg)

## Explanation
For each of the days of Hanukkah, a number of candles in the Menorah are lit, in a specific order. This little program uses javascript animation to illustrate the correct lighting order, and serves as a lively celebration of the Hanukkah tradition.


## Problem

There were several technical problems involved in getting this animation to work:
* Programming the movement of the candle used for lighting the others.
* Getting the transparency of the moving image right.
* Automating the calculation of which candles to light

## Solution

### Timing
I used the javascript `setInterval` function to repeatedly do one step of the animation, every 10 milliseconds. I used a second timing feature, `setTimeout` to repeat the entire animation after 500 milliseconds.

### Tasks
I broke up the animation into a number of parametrized "tasks" that had to be performed, and wrote code for each task:
* Initial: This task lifts the center, "lighting" candle and rotates it to get it in position to light the other candles.
* GoTo: This task smoothly moves the "lighting" candle to the position for lighting a particular candle.
* DropDown: This task lowers the "lighting" candle so that its flame is touching the wick of the candle to be lit next.
* RiseUp: This task raises the "lighting" candle so that it can be moved to the next candle.
* Final: This task puts the "lighting" candle back into place in the center and rotates it back to vertical.

To intialize the animation, a list of tasks that need to be done for each day is created, and during the execution, each task is performed and "popped" off the stack.

### Manipulating the "lighting" candle
Using the CSS "transform" property and javascript, I animated the tilting of the "lighting" candle from vertical to pointing down. An additional problem is that the candle flame is rotated, as well. That didn't look correct, because flames always rise vertically. To compensate, I made the flame into a separate HTML element, and rotated it in the opposite direction of the rotation of the candlestick, so that it remains vertical.

{% highlight javascript %}
function rotate() {
    deg++;
    
    document.getElementById("wholeCandle").style.transform = 'rotate('+deg+'deg)';
    document.getElementById("flame0").style.transform = 'rotate(-' + deg + 'deg)';
    if (deg > 90) {
        document.getElementById("wick").style.backgroundColor = "rgba(255,255,255,0.1)";
    } else {
        document.getElementById("wick").style.backgroundColor = "black";
    }
}

function unrotate() {
    deg--;
    
    document.getElementById("wholeCandle").style.transform = 'rotate('+deg+'deg)';
    document.getElementById("flame0").style.transform = 'rotate(-' + deg + 'deg)';
    if (deg > 90) {
        document.getElementById("wick").style.backgroundColor = "rgba(255,255,255,0.1)";
    } else {
        document.getElementById("wick").style.backgroundColor = "black";
    }
}
{% endhighlight %}

### Computing dates
I created an array containing the dates for Hannukah for a seven year period. Then I used the javascript `Date` class to find out the current date and decide whether it is currently Hanukkah, and which date. Then the appropriate animation is performed for that date.

### Transparency issues
There was a little bit of difficulty in making the right parts of the scene visible throughout the animation. The candle must block the background image, but since the candle flame is irregularly shaped, the background image must be visible through the gaps in the flame. The way that this was accomplished was by creating a "png" image for the flame. Using "png", you can specify which portions of an image should be "transparent".

I used an online image editor: [pixlr](https://pixlr.com/editor/) to create the flame image.

## Results
The resulting animation works very well. The animation is very smooth, and the web page is pretty smart in figuring out what the situation is (as far as which day of Hanukkah, etc.) For people to be able to view the animation at any time of the year (not just Hanukkah), I added controls to set the year and the day of Hanukkah to see what the animation would look like on that day.

The completed project can be viewed at [my website](http://dee-mccullough.com/hanukkah/)


## Conclusion

This started out to be a trivial little project. Originally, I tried creating animation by making lots of "snapshots" and sticking them together into an animated gif. But actually writing a program to do the animation seemed simpler. Carrying out the program and perfecting the way it looked and behaved turned out to be a good showcase for advanced HTML, Javascript and CSS.