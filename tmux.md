<img src = https://i.imgur.com/Ik46pbb.png height=500 weight=300>

# tmux


<img src = https://i.imgur.com/GYktm26.png>

tmux, the terminal multiplexer, is easily one of the most used tools by the Linux community (and not just pentesters!). While not a malicious tool, tmux makes running simultaneous tasks throughout a pentest incredibly easy. In this primer room, we'll walk through the process of installing and using some of the most common key combinations used in tmux. (Note, the installation process in this is geared towards Kali/Ubuntu.)


<img src=https://i.imgur.com/bL9Dn3U.png>

For another excellent resource on learning tmux, check out IppSec's video:[link](https://www.youtube.com/watch?v=Lqehvpe_djs)


### 1. First things first, let's go ahead and install tmux. This can be done on Ubuntu/Kali with the command: apt-get install tmux

### 2. Once tmux is installed, let's launch a new session. What command do we use to launch a new session without a custom name?

```tmux ```

### 3. All tmux commands start with a keyboard button combination. What is the first key in this combination?

```control ```

### 4. How about the second key? Note, these keys must be pressed at the same time and released before pressing the next target key in the combination.

```B ```

### 5. Lets go ahead and detach from our newly created tmux session. What key do we need to add to the combo in order to detach?

```D ```

### 6. Well shoot, we've detached from our session. How do we list all of our sessions?

```tmux ls ```


### 7. What did our session name default to when we created one without a set name?

```0 ```

### 8. Now that we've found the name of our session, how do we attach to it?

```tmux a -t 0 ```


### 9. Let's go ahead and make a new window in this session. What key do we add to the combo in order to do this?

```C ```

### 10. Seems like we have plenty of windows and nothing to fill them up with. Let's remedy that problem by deploying the VM above and running and nmap scan against it. Deploy the VM now. 

### 11. Run the following scan against the VM: nmap -sV -vv -sC TARGET_IP

### 12. Whew! Plenty of output to work with now! If you work with a relatively small terminal like me, this output might not all fit on screen at once. To fix that, let's enter 'copy mode'. What key do we add to the combo to enter copy mode?

```[ ```

### 13. Copy mode is very similar to 'less' and allows up to scroll up and down using the arrow keys. What if we want to go up to the very top?

```g ```


### 14. How about the bottom?

```G ```

### 15. What key do we press to exit 'copy mode'? 

```q ```

### 16. This window we're working in is nice and all but I think we need an upgrade. What key do we add to the combo to split the window vertically?

``` % ```

### 17. How about horizontally?

``` " ```

### 18. We can now move between these panes using the key combo and arrow keys, try it out!

### 19. We can also resize these panes by holding down the key combo and pressing the arrow keys, try it out now! 


### 20. Wait a minute, we've forgotten about our original window! We can go back it using the key combo and the number of the session! Try going back to this original window and then returning to our new one! 

### 21. Say one of these newly minted panes becomes unresponsive or we're just done working in it, what key do we add to the combo to 'kill' the pane?

```X ```

### 22. Now that's we've finished out work, what can we type to close the session? 

```exit ```

### 23. Last but now least, how do we spawn a named tmux session named 'neat'?

``` tmux new -s neat ```


