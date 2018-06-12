# VIM

VIM is a powerful text editor on command line environment which can be found on MacOS and almost all GNU/Linux distributions as a built-in software.

## Exit

OK Babaaa! For exit from vim just press `ESC` button, then press `:` and type `q!` and finally press enter! Why `ESC`? Vim has three modes:

1. **Normal Mode:** In this mode you can run commands, delete lines, fold sections and some actions like them. In Normal alphabets have meaning and do some semi-magical actions for you!
2. **Visual Mode:** In this mode you can select a part of text.
3. **Insert Mode:** You type on this mode!

You can switch to normal mode by pressing `ESC` in each two other modes! If you're already in normal mode nothing happens.

## Keys Meaning

As mentioned in previous section, in normal mode keys have meaning! What the mean of this? Well, the long story short: **You map an action to a key and combine them to make more complex actions, this is your real power!**. We discuss about combination of actions on next section. In this section we just have glim on keys and their meaning!

* `u`: **Undo** it! Don't Worry!

* `d`: **Delete**! It actually don't delete, it **cut** but if we don't paste it elsewhere, we can suppose that we deleted it! :D.
* `y`: **Yank**! It means **copy**!
* `p`: **Paste**! It paste things which you buffered (had copied or cut) **after** the cursor position.
* `P`: **Paste** but **before** the cursor position.

* `w`: **Forward** move on **word**s. For example on *hello, world from vim!*, if cursor is on *world* word, by pressing `w` it jumps to beginning of `from` word.
* `b`: **Backward** move on **word**s. With previous step, if now we press `b`, cursor move from *from* to `world`! Also if the cursor is on middle of a word (for example on *o* of *from*) by pressing `b`, it moves the cursor to the beginning of word.
* `l`: Move cursor **right**! I memorized it with this pattern: *I wanna move **r**ight, so I press **l**eft, I know, it doesn't make any sense but it works!
* `k`: Move cursor **top**!
* `j`: Move cursor **bottom**!
* `h`: Move cursor **right**! :D.
* `i`: Change mode to **insert** mode in **cursor position**.
* `I`: Change mode to **insert** mode in the **beginning of the line**.
* `a`: Change to insert mode and **append ** what you type from **next column** of current cursor position.
* `A`: **Append** but to **end of the line**.
* `o`: Change mode to **insert** mode in **next line**! You press `o`, it create a blank line after the current line and move the cursor to that.
* `O`: Change mode to **insert** mode in **previous line**! (this is big `o` not a zero :/).