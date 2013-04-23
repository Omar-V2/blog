---
layout: post
title: Editing Scala with vim
tags:
- vim
- scala
- ctags
type: post
published: true
---

Even though it hurts my brain a little bit, learning Scala has been
a pleasurable part of my job lately. It definitely isn't for the
faint-hearted but it has taught me a lot about thinking precisely about
the topics of immutability and shared state. The functional mindset forces 
you to think mainly about input and output of functions and this training, 
I feel, makes me a better programmer even when programming in a language other than Scala.

####Tools
I'm not really an IDE lover, quite the opposite, but I have started taking 
a shine to the comforts that Intelli-J is offering when writing Java.

I was happy to see that Intelli-J has a plugin for Scala development and have
been using that for the last couple of months. Sadly, the plugin is really,
really slow which makes it pretty unusable to me.

There is no question that the languages is beautiful but the tools aren't.

####Enter vim

I'm already using vim for everything other than Java so I hunted around the
web in order to find plugins. I've been quite satisfied with the
following combination.

####[vim-scala](vim-scala)
Obviously, you will need syntax highlighting and this plugin provides that.

####[neocomplcache](https://github.com/Shougo/neocomplcache)

The built-in vim autocomplete isn't great for Scala but the [`neocomplcache`](https://github.com/Shougo/neocomplcache) plugin
makes typing out long Scala class or package names easier. There is no
semantic analysis behind it so all this plugin does is string matching but
I was very surprised to realise that this takes you a long way. To me at least
it is adequate.

####ctags

In order to enable jump-to-source navigation you need to configure ctags to
index scala files. You can do that by adding the following to your `~/.ctags`:

```bash
--langdef=scala
--langmap=scala:.scala
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*(private|protected)?[ \t]*class[ \t]+([a-zA-Z0-9_]+)/\4/c,classes/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*(private|protected)?[ \t]*object[ \t]+([a-zA-Z0-9_]+)/\4/c,objects/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*(private|protected)?[ \t]*case class[ \t]+([a-zA-Z0-9_]+)/\4/c,case classes/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*(private|protected)?[ \t]*case object[ \t]+([a-zA-Z0-9_]+)/\4/c,case objects/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*(private|protected)?[ \t]*trait[ \t]+([a-zA-Z0-9_]+)/\4/t,traits/
--regex-scala=/^[ \t]*type[ \t]+([a-zA-Z0-9_]+)/\1/T,types/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*def[ \t]+([a-zA-Z0-9_]+)/\3/m,methods/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*val[ \t]+([a-zA-Z0-9_]+)/\3/l,constants/
--regex-scala=/^[ \t]*((abstract|final|sealed|implicit|lazy)[ \t]*)*var[ \t]+([a-zA-Z0-9_]+)/\3/l,variables/
--regex-scala=/^[ \t]*package[ \t]+([a-zA-Z0-9_.]+)/\1/p,packages/
```

After having done that you need to run ctags from the root of your project
directory with something like this:

```bash
ctags -R . --exclude=target --exclude=vendor
```

By default vim will only look for the tags file in the directory of the file
open in the current buffer. If you want vim do move up the directory hierarchy
until it has found the file add this to your `~/.vimrc`:

```vim
set tags=tags;/
```

Now, if you move the cursor over a type name and press `Ctrl-]` you will jump
to the definition of the type. That's pretty nifty!

If you want to go back to where you were before the jump press `Ctrl-T`.