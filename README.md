# Better Window names for tmux

A plugin to name your tmux windows smartly, like IDE's.

![Tmux Window Name Screenshot](screenshots/example.png)

## Dependencies

* tmux (Tested on 3.0a)
* Python 3.6.8+ (Maybe can be lower, tested on 3.6.8)
* [libtmux](https://github.com/tmux-python/libtmux) (will be installed during plugin init)

## Usage

If you are using tmux as your main multiplexer you probably found yourself with 5+ windows per session with indexed names but no information about whats going on in the windows.

You tried to configure `automatic-rename` and `automatic-rename-format` but you found yourself pretty limited.

This plugin comes to solve those issues to name your windows inspired by IDE tablines.

Make sure to check out [Examples](#Examples).

### How it works
Each time you unfocus from a pane, the plugin looks for every active pane in your session windows.

1. If shell is running, it shows the current dir as short as possible, `long_dir/a` -> `a`, it avoids [intersections](#Intersections) too!
1. If "regular" program is running it shows the program with the args, `less ~/my_file` -> `less ~/my_file`.
1. If "special" program is running it shows the program with the dir attached, `git diff` (in `long_dir/a`) -> `git diff:a`, it avoids [intersections](#Intersections) too!

### Intersections

To make the shortest path as possible the plugin finds the shortest not common path if your windows.

#### Examples
This session:
```
1. ~/workspace/my_project
2. ~/workspace/my_project/tests/
3. ~/workspace/my_other_project
4. ~/workspace/my_other_project/tests
```
Will display:
```
1. my_project
2. my_project/tests
3. my_other_project
4. my_other_project/tests
```

---

It knows which programs runs
```
1. ~/workspace/my_project (with nvim)
2. ~/workspace/my_project
3. ~/workspace/my_other_project (with git diff)
4. ~/workspace/my_other_project
```
Will display:
```
1. nvim:my_project
2. my_project
3. git diff:my_other_project
4. my_other_project
```

For more scenarios you check out the [tests](tests/test_exclusive_paths.py).

--- 

## Installation

### Installation with [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm) (recommended)

Add plugin to the list of TPM plugins:

```tmux.conf
set -g @plugin 'ofirgall/tmux-window-name'
```

Press prefix + I to install it.

### Manual Installation

Clone the repo:

```bash
$ git clone https://github.com/ofirgall/tmux-window-name.git ~/clone/path
```

Add this line to your .tmux.conf:

```tmux.conf
run-shell ~/clone/path/tmux-window-name.tmux
```

Reload TMUX environment with:

```bash
$ tmux source-file ~/.tmux.conf
```

## Configuration Options

TODO: complete
### `@new_browser_window`

The command to run a new window.
E.g: `firefox --new-window url`

```tmux.conf
set -g @new_browser_window 'firefox --new-window'
```

---

# Testing
Run `pytest` at the root dir

---

## License

[MIT](LICENSE)
