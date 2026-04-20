# Launch & Domains

Source: <https://wezterm.org/config/launch.html>.

## Default shell

```lua
config.default_prog = { '/opt/homebrew/bin/fish', '-l' }
config.default_cwd  = '/Users/you/work'
```

## Environment

```lua
config.set_environment_variables = {
  EDITOR = 'nvim',
  PATH = '/opt/homebrew/bin:' .. os.getenv('PATH'),
}
```

## Launcher menu

Accessed via the `+` tab button or `ShowLauncher` / `ShowLauncherArgs` action.

```lua
config.launch_menu = {
  { label = 'zsh',    args = { 'zsh', '-l' } },
  { label = 'fish',   args = { 'fish', '-l' } },
  { label = 'python', args = { 'python3' } },
  { label = 'ssh prod',
    args = { 'ssh', 'prod.example.com' },
    cwd  = '/tmp',
    set_environment_variables = { MY_FLAG = '1' } },
}
```
SpawnCommand fields: `label`, `args`, `cwd`, `domain`, `set_environment_variables`.

## Remote / multiplexed domains

### SSH
```lua
config.ssh_domains = {
  { name = 'prod',
    remote_address = 'prod.example.com',
    username = 'ubuntu',
    multiplexing = 'None', -- or 'WezTerm'
    assume_shell = 'Posix',
  },
}
```

### WSL (Windows)
```lua
config.wsl_domains = {
  { name = 'WSL:Ubuntu', distribution = 'Ubuntu',
    default_cwd = '/home/you', default_prog = {'bash','-l'} },
}
```

### Unix sockets (local multiplexing)
```lua
config.unix_domains = {
  { name = 'unix',
    socket_path = '/tmp/wezterm.sock' },
}
config.default_gui_startup_args = { 'connect', 'unix' }
```

### Serial
```lua
config.serial_ports = {
  { name = 'pi', port = '/dev/tty.usbserial', baud = 115200 },
}
```

## Exit behavior

- `exit_behavior = 'Close' | 'Hold' | 'CloseOnCleanExit'`
- `clean_exit_codes = { 0, 130 }` — treat these as clean
- `skip_close_confirmation_for_processes_named = { 'bash','zsh','fish' }`

## Reminder
Changes to `default_prog`, `default_cwd`, `set_environment_variables`, domain configs apply **only to newly spawned panes/tabs** — existing ones keep their environment.
