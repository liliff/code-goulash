### wonder what the acronym was supposed to mean

Compile and maybe copy somewhere:

```
cargo build --release && cp target/release/mws2vddgpupt ~/bin/
```

Figure out the display ID for the screen you want to disable using `xrandr`.

Add a libvirt hook to automatically run it on VM launch by editing
`/etc/libvirt/hooks/qemu`, e.g. to watch the `windows` VM and disable DP-2:

```
#!/bin/bash 
if [[ $1 == "windows" ]] && [[ $2 == "start" ]]
then
  su - <USER> -c "tmux new -d 'DISPLAY=:0 /home/<USER>/bin/mws2vddgpupt -d 10 DP-2 windows'"
fi
```

Replace `<USER>` with your desktop user (and change the path to the binary).
(This will start a tmux session and run the script, so that your VM doesn't
wait for the program to quit before starting.)

The `-d N` option will introduce N seconds of delay before disabling the
screen, which is useful if your display automatically switches inputs to
another active input when one input goes away, since it gives the VM some time
to activate its own display output.
