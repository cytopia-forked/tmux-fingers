# tmux-fingers

[![CircleCI](https://circleci.com/gh/Morantron/tmux-fingers.svg?style=svg)](https://circleci.com/gh/Morantron/tmux-fingers)

**tmux-fingers**: copy pasting with vimium/vimperator like hints.

![yay](http://i.imgur.com/5bSrBew.gif)

# Usage

Press ( `prefix + F` ) to enter **[fingers]** mode, it will highlight relevant stuff in the current
pane along with letter hints. By pressing those letters, the highlighted match
will be yanked. Less keystrokes == profit!

Relevant stuff:

* File paths
* git SHAs
* numbers ( 4+ digits )
* urls
* ip addresses

It also works on copy mode, but requires *tmux 2.2* or newer to properly take
the scroll position into account.

Additionally, you can install
[tmux-yank](https://github.com/tmux-plugins/tmux-yank) for system clipboard
integration.

## Key shortcuts

While in **[fingers]** mode, you can use the following shortcuts:

* `a-z`: yank a highlighted hint.
* `<space>`: toggle compact hints ( see [@fingers-compact-hints](#fingers-compact-hints) ).
* `<Ctrl-C>`: exit **[fingers]** mode
* `<esc>`: exit help or **[fingers]** mode
* `?`: show help.

# Requirements

* bash 4+
* tmux 2.1+ ( 2.2 recommended )
* gawk ( optional but recommended )

# Installation

## Using [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm)

Add the following to your list of TPM plugins in `.tmux.conf`:

```
set -g @plugin 'Morantron/tmux-fingers'
```

Hit `prefix + I` to fetch and source the plugin. You should now be able to use
the plugin!

## Manual

Clone the repo:

```
➜ git clone https://github.com/Morantron/tmux-fingers ~/clone/path
```

Source it in your `.tmux.conf`:

```
run-shell ~/clone/path/tmux-fingers.tmux
```

Reload TMUX conf by running:

```
➜ tmux source-file ~/.tmux.conf
```

# Configuration

You can change the key that invokes **tmux-fingers**:

## @fingers-key

`default: F`

Customize how to enter copy mode. Always preceded by prefix: `prefix + @fingers-key`

```
set -g @fingers-key F
```

## @fingers-patterns-N

You can also add additional patterns if you want more stuff to be highlighted:

```
set -g @fingers-pattern-0 'git rebase --(abort|continue)'
set -g @fingers-pattern-1 'yolo'
.
.
.
set -g @fingers-pattern-50 'whatever'
```

Patterns are case insensitive, and grep's extended syntax ( ERE ) should be used.
`man grep` for more info.

If the introduced regexp contains an error, an error will be shown when
invoking the plugin.

It's recommended to install `gawk` for better support of custom
patterns: some things like interval expressions do not work in built-in `awk`
in OSX/BSD systems.

## @fingers-copy-command

By default **tmux-fingers** will just yank matches using tmux clipboard ( or
[tmux-yank](https://github.com/tmux-plugins/tmux-yank) if present ).

If you still want to set your own custom command you can do so like this:

```
set -g @fingers-copy-command 'xclip -selection clipboard'
```

## @fingers-compact-hints

`default: 1`

By default **tmux-fingers** will show hints a compact format. For example:

<pre>
/path/to/foo/bar/lol

<i>with <bold>@fingers-compact-hints</bold> set to <bold>1</bold>:</i>

<strong>aw</strong>ath/to/foo/bar/lol

<i>with <bold>@fingers-compact-hints</bold> set to <bold>0</bold>:</i>

/path/to/foo/bar/lol <strong>[aw]</strong>
</pre>

( _pressing *aw* would yank `/path/to/foo/bar/lol`_ )

While in **[fingers]** mode you can press `<space>` to toggle compact mode on/off.

Compact mode is preferred because it preserves the length of lines and doesn't
cause line wraps, making it easier to follow.

However for small hints this can be troublesome: a path as small as `/a/b`
would have half of its original content concealed. If that's the case you can
quickly toggle off compact mode by pressing `<space>`.

# Acknowledgements and inspiration

This plugin is heavily inspired by
[tmux-copycat](https://github.com/tmux-plugins/tmux-copycat) ( **tmux-fingers**
predefined search are *copycatted* :trollface: from
[tmux-copycat](https://github.com/tmux-plugins/tmux-copycat) ).

Kudos to [bruno-](https://github.com/bruno-) for paving the way to tmux
plugins! :clap: :clap:

# License

[MIT](https://github.com/Morantron/tmux-fingers/blob/master/LICENSE)
