# Validate and Format SD Cards

First create a new paritition with type **0x0c** using fdisk. Then:

```bash
# Format
sudo mkfs.vfat -F 32 -S 4096 -n BLACKVUE /dev/sdg1

# Mount
sudo mount /dev/sdg1 /mnt/usb0 -o "rw,noatime,uid=$(id -u),gid=$(id -g)"

# Validate Capacity
screen -S usb0 -dm bash -c '
    set -x
    mountpoint /mnt/usb0 && f3write $_ && f3read $_
    set +x
    exec bash
'
```

The `exec bash` part causes the screen session to persist when everything is done instead of terminating and losing the
output.
