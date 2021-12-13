# How to persist a specific HDMI output

GNOME 40, PipeWire with the pipewire-pulse "adapter," and an NVIDIA GTX 970 card.

## Observations

I test for audio using `speaker-test -b1000`.

When audio is working through the HDMI for DELL U3415W, `pactl list sinks` reports:

- ALSA card 1
- ALSA device 7
- ALSA subdevice 0

After I disconnect the HDMI cable and re-connect it, the audio is _not_ working, and `pactl list links` reports:

- ALSA card 1
- ALSA device 3
- ALSA subdevice 0

What's interesting is that this sink is the only one listed. It happens to be the sink for the first HDMI output, which goes to another monitor. So at this point, I can't even select a different sink. How do I make available the sink that I need?

According to `pactl list sinks`, it looks like the card stays the same, but a different "profile" is chosen:

```sh
Card #36
    Name: alsa_card.pci-0000_07_00.1
    Driver: alsa
    Owner Module: n/a
    Properties:
        device.enum.api = "udev"
        device.api = "alsa"
        media.class = "Audio/Device"
        api.alsa.path = "hw:1"
        api.alsa.card = "1"
        api.alsa.card.name = "HDA NVidia"
        api.alsa.card.longname = "HDA NVidia at 0xfc080000 irq 133"
        device.plugged.usec = "163805001"
        device.bus_path = "pci-0000:07:00.1"
        device.sysfs.path = "/sys/devices/pci0000:00/0000:00:03.1/0000:07:00.1/sound/card1"
        device.bus = "pci"
        device.subsystem = "sound"
        device.vendor.id = "4318"
        device.vendor.name = "NVIDIA Corporation"
        device.product.id = "4027"
        device.product.name = "GM204 High Definition Audio Controller"
        device.name = "alsa_card.pci-0000_07_00.1"
        device.description = "GM204 High Definition Audio Controller"
        device.nick = "HDA NVidia"
        device.icon_name = "audio-card-pci"
        api.alsa.use-acp = "true"
        api.acp.auto-profile = "false"
        api.acp.auto-port = "false"
        api.dbus.ReserveDevice1 = "Audio1"
        factory.id = "14"
        client.id = "31"
        object.id = "36"
        object.path = "alsa:pcm:1"
        alsa.card = "1"
        alsa.card_name = "HDA NVidia"
        alsa.long_card_name = "HDA NVidia at 0xfc080000 irq 133"
        alsa.driver_name = "snd_hda_intel"
        device.string = "1"
    Profiles:
        off: Off (sinks: 0, sources: 0, priority: 0, available: yes)
        output:hdmi-stereo: Digital Stereo (HDMI) Output (sinks: 1, sources: 0, priority: 5900, available: yes)
        output:hdmi-stereo-extra1: Digital Stereo (HDMI 2) Output (sinks: 1, sources: 0, priority: 5700, available: yes)
        output:hdmi-stereo-extra2: Digital Stereo (HDMI 3) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra3: Digital Stereo (HDMI 4) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra4: Digital Stereo (HDMI 5) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra5: Digital Stereo (HDMI 6) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra6: Digital Stereo (HDMI 7) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-surround-extra2: Digital Surround 5.1 (HDMI 3) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra2: Digital Surround 7.1 (HDMI 3) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra3: Digital Surround 5.1 (HDMI 4) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra3: Digital Surround 7.1 (HDMI 4) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra4: Digital Surround 5.1 (HDMI 5) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra4: Digital Surround 7.1 (HDMI 5) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra5: Digital Surround 5.1 (HDMI 6) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra5: Digital Surround 7.1 (HDMI 6) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra6: Digital Surround 5.1 (HDMI 7) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra6: Digital Surround 7.1 (HDMI 7) Output (sinks: 1, sources: 0, priority: 600, available: no)
        pro-audio: Pro Audio (sinks: 7, sources: 0, priority: 1, available: yes)
    Active Profile: output:hdmi-stereo
```

Later, I change to the working output in GNOME settings, confirm the audio output, and observe that a different profile is selected:

```sh
    Profiles:
        off: Off (sinks: 0, sources: 0, priority: 0, available: yes)
        output:hdmi-stereo: Digital Stereo (HDMI) Output (sinks: 1, sources: 0, priority: 5900, available: yes)
        output:hdmi-stereo-extra1: Digital Stereo (HDMI 2) Output (sinks: 1, sources: 0, priority: 5700, available: yes)
        output:hdmi-stereo-extra2: Digital Stereo (HDMI 3) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra3: Digital Stereo (HDMI 4) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra4: Digital Stereo (HDMI 5) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra5: Digital Stereo (HDMI 6) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-stereo-extra6: Digital Stereo (HDMI 7) Output (sinks: 1, sources: 0, priority: 5700, available: no)
        output:hdmi-surround-extra2: Digital Surround 5.1 (HDMI 3) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra2: Digital Surround 7.1 (HDMI 3) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra3: Digital Surround 5.1 (HDMI 4) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra3: Digital Surround 7.1 (HDMI 4) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra4: Digital Surround 5.1 (HDMI 5) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra4: Digital Surround 7.1 (HDMI 5) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra5: Digital Surround 5.1 (HDMI 6) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra5: Digital Surround 7.1 (HDMI 6) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround-extra6: Digital Surround 5.1 (HDMI 7) Output (sinks: 1, sources: 0, priority: 600, available: no)
        output:hdmi-surround71-extra6: Digital Surround 7.1 (HDMI 7) Output (sinks: 1, sources: 0, priority: 600, available: no)
        pro-audio: Pro Audio (sinks: 7, sources: 0, priority: 1, available: yes)
    Active Profile: output:hdmi-stereo-extra1
```

After this change, `pactl list sinks` reports only a sink for the second HDMI output, which is the one I want.

## Hypothesis

I can control the sink that is available by default by controlling which profile is has the highest priority.

## Test

I disconnect the HDMI cable and re-connect it.
I verify that `speaker-test` outputs no audio.
I run `pactl set-card-profile "alsa_card.pci-0000_07_00.1" "output:hdmi-stereo-extra1"`.
I verify that `speaker-test` now outputs audio!

Now I need to make sure this profile is chosen whenever the HDMI cable is connected.

When I disconnect the HDMI cable, the profile becomes unavailable, and is no longer the default.
When I re-connect the HDMI cable, the default profile is not changed.

Options:
1. Change the priority of the profiles, so that the one I want is always chosen, if it is available.
2. Update the card profile in response to the HDMI cable being connected.
3. Something else?

I have no idea how to do (1). From what I read, it looks like PulseAudio profiles are opaque, and come entirely from ALSA.

> A card profile represents an opaque configuration set of a card, like an analog or digital mode. It defines the backend-specific configuration of the card, and the list of currently available device ports and devices.
>
> ALSA backend in PulseAudio automatically creates PulseAudio cards, card profiles, device ports, sources, and sinks:
>
> PulseAudio card profile is associated with a configuration set for an ALSA card. It defines a subset of ALSA devices belonging to a card, and so the list of available device ports, sources, and sinks.
>
> -- https://gavv.github.io/articles/pulseaudio-under-the-hood/

Looks like this is controlled by a card profile, described in https://www.freedesktop.org/wiki/Software/PulseAudio/Backends/ALSA/Profiles/.

I found a default set of profiles on disk, and upstream https://cgit.freedesktop.org/pulseaudio/pulseaudio/tree/src/modules/alsa/mixer/profile-sets/default.conf. There are some HDMI profiles that match by name, and the "HDMI 2" profile that I want to prioritize gets a lower priority from this default profile:

```ini
[Mapping hdmi-stereo]
description = Digital Stereo (HDMI)
device-strings = hdmi:%f
paths-output = hdmi-output-0
channel-map = left,right
priority = 9
direction = output

[Mapping hdmi-stereo-extra1]
description = Digital Stereo (HDMI 2)
device-strings = hdmi:%f,1
paths-output = hdmi-output-1
channel-map = left,right
priority = 7
direction = output
```

I think my best bet might be to write a custom profile for the NVidia card I have.

I also found something concerning in the alsa-state.service logs. Since I upgraded to Fedora 34, where Pipewire is used by default, ALSA complains about "UCM not supported" for all my cards. I don't know what this error implies. It might even be ["harmeless"](https://gitlab.freedesktop.org/pipewire/pipewire/-/issues/1269#note_959456).

```log
- Boot dd2f0f721e0d4f9db8a9a94337e19fbc --
Oct 23 13:20:10 bowfin systemd[1]: Started Manage Sound Card State (restore and store).
Oct 23 13:20:10 bowfin alsactl[3582]: alsactl 1.2.4 daemon started
Oct 23 15:37:57 bowfin systemd[1]: Stopping Manage Sound Card State (restore and store)...
Oct 23 15:37:57 bowfin alsactl[3582]: alsactl daemon stopped
Oct 23 15:37:57 bowfin systemd[1]: alsa-state.service: Succeeded.
Oct 23 15:37:57 bowfin systemd[1]: Stopped Manage Sound Card State (restore and store).
-- Boot 8b4f7021d4f14355bcb4eccab889dc58 --
Oct 23 15:40:14 bowfin systemd[1]: Started Manage Sound Card State (restore and store).
Oct 23 15:40:14 bowfin alsactl[1346]: alsactl 1.2.4 daemon started
Oct 23 15:52:20 bowfin systemd[1]: Stopping Manage Sound Card State (restore and store)...
Oct 23 15:52:20 bowfin alsactl[1346]: alsactl daemon stopped
Oct 23 15:52:20 bowfin systemd[1]: alsa-state.service: Deactivated successfully.
Oct 23 15:52:20 bowfin systemd[1]: Stopped Manage Sound Card State (restore and store).
Oct 23 15:52:20 bowfin systemd[1]: Started Manage Sound Card State (restore and store).
Oct 23 15:52:20 bowfin alsactl[31996]: alsactl 1.2.5.1 daemon started
Oct 23 15:52:20 bowfin alsactl[31996]: alsa-lib main.c:1405:(snd_use_case_mgr_open) error: failed to import hw:0 use case configuration -2
Oct 23 15:52:20 bowfin alsactl[31996]: alsa-lib parser.c:242:(error_node) UCM is not supported for this HDA model (HDA NVidia at 0xfc080000 irq 133)
Oct 23 15:52:20 bowfin alsactl[31996]: alsa-lib main.c:1405:(snd_use_case_mgr_open) error: failed to import hw:1 use case configuration -6
Oct 23 15:52:20 bowfin alsactl[31996]: alsa-lib parser.c:242:(error_node) UCM is not supported for this HDA model (HD-Audio Generic at 0xfc800000 irq 135)
Oct 23 15:52:20 bowfin alsactl[31996]: alsa-lib main.c:1405:(snd_use_case_mgr_open) error: failed to import hw:2 use case configuration -6
Oct 23 15:57:01 bowfin systemd[1]: Stopping Manage Sound Card State (restore and store)...
Oct 23 15:57:01 bowfin systemd[1]: alsa-state.service: Deactivated successfully.
Oct 23 15:57:01 bowfin systemd[1]: Stopped Manage Sound Card State (restore and store).
```

## Plan

1.  Create udev rule

    I'm not sure what the rule should be for. Here are the udev events when I disconnect and re-connect the HDMI cable.

    ```sh
    > udevadm monitor
    monitor will print the received events for:
    UDEV - the event which udev sends out after rule processing
    KERNEL - the kernel uevent

    KERNEL[21656.741995] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    UDEV  [21656.743693] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    KERNEL[21656.928907] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    UDEV  [21656.929548] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    KERNEL[21660.487555] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    UDEV  [21660.488870] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    KERNEL[21660.674474] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    UDEV  [21660.675092] change   /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0 (drm)
    ```

    Here's how to discover the attributes of the card:

    ```sh
    > udevadm info --attribute-walk -p  /devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0

    Udevadm info starts with the device specified by the devpath and then
    walks up the chain of parent devices. It prints for every device
    found, all possible attributes in the udev rules key format.
    A rule to match, can be composed by the attributes of the device
    and the attributes from one single parent device.

    looking at device '/devices/pci0000:00/0000:00:03.1/0000:07:00.0/drm/card0':
            KERNEL=="card0"
            SUBSYSTEM=="drm"
            DRIVER==""
            ATTR{power/control}=="auto"
            ATTR{power/runtime_active_time}=="0"
            ATTR{power/runtime_status}=="unsupported"
            ATTR{power/runtime_suspended_time}=="0"

    looking at parent device '/devices/pci0000:00/0000:00:03.1/0000:07:00.0':
            KERNELS=="0000:07:00.0"
            SUBSYSTEMS=="pci"
            DRIVERS=="nvidia"
            ATTRS{ari_enabled}=="0"
            ATTRS{boot_vga}=="1"
            ATTRS{broken_parity_status}=="0"
            ATTRS{class}=="0x030000"
            ATTRS{consistent_dma_mask_bits}=="40"
            ATTRS{current_link_speed}=="2.5 GT/s PCIe"
            ATTRS{current_link_width}=="16"
            ATTRS{d3cold_allowed}=="1"
            ATTRS{device}=="0x13c2"
            ATTRS{dma_mask_bits}=="40"
            ATTRS{driver_override}=="(null)"
            ATTRS{enable}=="1"
            ATTRS{irq}=="136"
            ATTRS{link/clkpm}=="0"
            ATTRS{link/l1_1_aspm}=="0"
            ATTRS{link/l1_1_pcipm}=="0"
            ATTRS{link/l1_aspm}=="0"
            ATTRS{local_cpulist}=="0-23"
            ATTRS{local_cpus}=="00ffffff"
            ATTRS{max_link_speed}=="8.0 GT/s PCIe"
            ATTRS{max_link_width}=="16"
            ATTRS{msi_bus}=="1"
            ATTRS{msi_irqs/136}=="msi"
            ATTRS{numa_node}=="-1"
            ATTRS{power/control}=="auto"
            ATTRS{power/runtime_active_time}=="22093641"
            ATTRS{power/runtime_status}=="active"
            ATTRS{power/runtime_suspended_time}=="0"
            ATTRS{power/wakeup}=="disabled"
            ATTRS{power/wakeup_abort_count}==""
            ATTRS{power/wakeup_active}==""
            ATTRS{power/wakeup_active_count}==""
            ATTRS{power/wakeup_count}==""
            ATTRS{power/wakeup_expire_count}==""
            ATTRS{power/wakeup_last_time_ms}==""
            ATTRS{power/wakeup_max_time_ms}==""
            ATTRS{power/wakeup_total_time_ms}==""
            ATTRS{power_state}=="D0"
            ATTRS{reset_method}=="bus"
            ATTRS{revision}=="0xa1"
            ATTRS{subsystem_device}=="0x8508"
            ATTRS{subsystem_vendor}=="0x1043"
            ATTRS{vendor}=="0x10de"

    looking at parent device '/devices/pci0000:00/0000:00:03.1':
            KERNELS=="0000:00:03.1"
            SUBSYSTEMS=="pci"
            DRIVERS=="pcieport"
            ATTRS{aer_rootport_total_err_cor}=="0"
            ATTRS{aer_rootport_total_err_fatal}=="0"
            ATTRS{aer_rootport_total_err_nonfatal}=="0"
            ATTRS{ari_enabled}=="0"
            ATTRS{broken_parity_status}=="0"
            ATTRS{class}=="0x060400"
            ATTRS{consistent_dma_mask_bits}=="32"
            ATTRS{current_link_speed}=="2.5 GT/s PCIe"
            ATTRS{current_link_width}=="16"
            ATTRS{d3cold_allowed}=="1"
            ATTRS{device}=="0x1483"
            ATTRS{dma_mask_bits}=="32"
            ATTRS{driver_override}=="(null)"
            ATTRS{enable}=="1"
            ATTRS{irq}=="28"
            ATTRS{local_cpulist}=="0-23"
            ATTRS{local_cpus}=="00ffffff"
            ATTRS{max_link_speed}=="16.0 GT/s PCIe"
            ATTRS{max_link_width}=="16"
            ATTRS{msi_bus}=="1"
            ATTRS{msi_irqs/28}=="msi"
            ATTRS{numa_node}=="-1"
            ATTRS{power/autosuspend_delay_ms}=="100"
            ATTRS{power/control}=="auto"
            ATTRS{power/runtime_active_time}=="22093646"
            ATTRS{power/runtime_status}=="active"
            ATTRS{power/runtime_suspended_time}=="0"
            ATTRS{power/wakeup}=="disabled"
            ATTRS{power/wakeup_abort_count}==""
            ATTRS{power/wakeup_active}==""
            ATTRS{power/wakeup_active_count}==""
            ATTRS{power/wakeup_count}==""
            ATTRS{power/wakeup_expire_count}==""
            ATTRS{power/wakeup_last_time_ms}==""
            ATTRS{power/wakeup_max_time_ms}==""
            ATTRS{power/wakeup_total_time_ms}==""
            ATTRS{power_state}=="D0"
            ATTRS{reset_method}=="pm"
            ATTRS{revision}=="0x00"
            ATTRS{secondary_bus_number}=="7"
            ATTRS{subordinate_bus_number}=="7"
            ATTRS{subsystem_device}=="0x8808"
            ATTRS{subsystem_vendor}=="0x1043"
            ATTRS{vendor}=="0x1022"

    looking at parent device '/devices/pci0000:00':
            KERNELS=="pci0000:00"
            SUBSYSTEMS==""
            DRIVERS==""
            ATTRS{power/control}=="auto"
            ATTRS{power/runtime_active_time}=="0"
            ATTRS{power/runtime_status}=="unsupported"
            ATTRS{power/runtime_suspended_time}=="0"
            ATTRS{waiting_for_supplier}=="0"


    ```

I tried creating the udev rule, profile set, and paths, but couldn't get that to work.

However, I did get the following to work:

1.  Lowered the priority of the "hdmi-stereo" Mapping in the default.conf profile set to below the hdmi-stereo-extra1 priority.

    Lowering the priority from 9 to 1 in the `default.conf` profile set file changed the priority reported by pactl from 5900 to 5100:

    ```txt
    Profiles:
            off: Off (sinks: 0, sources: 0, priority: 0, available: yes)
            output:hdmi-stereo-extra1: Digital Stereo (HDMI 2) Output (sinks: 1, sources: 0, priority: 5700, available: yes)
            output:hdmi-stereo-extra2: Digital Stereo (HDMI 3) Output (sinks: 1, sources: 0, priority: 5700, available: no)
            output:hdmi-stereo-extra3: Digital Stereo (HDMI 4) Output (sinks: 1, sources: 0, priority: 5700, available: no)
            output:hdmi-stereo-extra4: Digital Stereo (HDMI 5) Output (sinks: 1, sources: 0, priority: 5700, available: no)
            output:hdmi-stereo-extra5: Digital Stereo (HDMI 6) Output (sinks: 1, sources: 0, priority: 5700, available: no)
            output:hdmi-stereo-extra6: Digital Stereo (HDMI 7) Output (sinks: 1, sources: 0, priority: 5700, available: no)
            output:hdmi-stereo: Digital Stereo (HDMI) Output (sinks: 1, sources: 0, priority: 5100, available: yes)
    ```

    Lowering the priority from 59 to 1 in the `hdmi-output-0.conf` path file had no effect! This might be because, since upgrading to Fedora 34, ALSA says it can't use UCM with either of the audio cards. I really don't know.

2.  Restarted both pipewire and pipewire-pulse services: `systemctl --user restart pipewire.service pipewire-pulse.service`

This approach will break as soon as the `default.conf` profile set file is updated, e.g., by a package upgrade.
