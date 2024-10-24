# Using pipx and pyenv together

I had issues after upgrading Python on my Mac with brew. The new 3.13.x didn't work with many packages. For example, [llm](https://github.com/simonw/llm) worked, but the plugin [llm-claude-3](https://github.com/simonw/llm-claude-3) didn't.

The gcloud CLI also stopped working because it only supports Python 3.12 and older.

My fix:

Install a specific version of python with pyenv and use pipx to install python binaries in isolated environments.

1. Install Python 3.12 using pyenv
2. Install pipx
3. Set pipx to use the pyenv Python version (add to my `.zshenv`):

```sh
export PIPX_DEFAULT_PYTHON="$HOME/.pyenv/versions/3.12.5/bin/python3"
```

4. Use pipx to install `llm`:

```
pipx install llm
```
