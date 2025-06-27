# ai_interface

## uv

### install

1. windows

```shell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

2. linux
Use curl to download the script and execute it with sh:

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

If your system doesn't have curl, you can use wget:

```shell
wget -qO- https://astral.sh/uv/install.sh | sh
```

### uninstall

```shell
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
```
