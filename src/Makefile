
DEVICE_GOWINPACK=GW2A-18C

DEVICE_NEXTPNR=GW2AR-LV18QN88C8/I7
FAMILY_NEXTPNR=GW2A-18C
CONSTRAINTS=tang/nano20k/nanomig_lcd.cst

MACRO_YOSYS=

YOSYS=yosys
NEXTPNR=nextpnr-himbaechel
GOWINPACK=gowin_pack

SOURCES=\
        fx68k/fx68k.v \
        fx68k/fx68kAlu.v \
        fx68k/uaddrPla.v \
        fx68k/fx68k_MicroRom.v \
        fx68k/fx68k_NanoRom.v \
        minimig/Amber.v \
        minimig/Agnus.v \
		nanomig.v \
		minimig-aga/amiga_clk.v \
		minimig-aga/cpu_wrapper.v \
		minimig-aga/minimig.v \
		minimig-aga/ciaa.v \
		minimig-aga/ciab.v \
		minimig-aga/cia_int.v \
		minimig-aga/cia_timera.v \
		minimig-aga/cia_timerb.v \
		minimig-aga/cia_timerd.v \
		minimig-aga/paula.v \
		minimig-aga/paula_uart.v \
		minimig-aga/paula_audio_channel.v \
		minimig-aga/paula_audio_mixer.v \
		minimig-aga/paula_audio.v \
		minimig-aga/paula_audio_volume.v \
		minimig-aga/paula_floppy_fifo.v \
		minimig-aga/paula_floppy.v \
		minimig-aga/paula_intcontroller.v \
		minimig-aga/agnus.v \
		minimig-aga/agnus_audiodma.v \
		minimig-aga/agnus_blitter_adrgen.v \
		minimig-aga/agnus_blitter_minterm.v \
		minimig-aga/agnus_diskdma.v \
		minimig-aga/agnus_beamcounter.v \
		minimig-aga/agnus_blitter_barrelshifter.v \
		minimig-aga/agnus_blitter.v \
		minimig-aga/agnus_refresh.v \
		minimig-aga/agnus_bitplanedma.v \
		minimig-aga/agnus_blitter_fill.v \
		minimig-aga/agnus_copper.v \
		minimig-aga/agnus_spritedma.v \
		minimig-aga/denise.v \
		minimig-aga/denise_bitplane_shifter.v \
		minimig-aga/denise_collision.v \
		minimig-aga/denise_colortable.v \
		minimig-aga/denise_playfields.v \
		minimig-aga/denise_sprites_shifter.v \
		minimig-aga/denise_bitplanes.v \
		minimig-aga/denise_hamgenerator.v \
		minimig-aga/denise_spritepriority.v \
		minimig-aga/denise_sprites.v \
		minimig-aga/denise_colortable_ram_mf.v \
		minimig-aga/gary.v \
		minimig-aga/gayle.v \
		minimig-aga/ide.v \
		minimig-aga/minimig_m68k_bridge.v \
		minimig-aga/minimig_bankmapper.v \
		minimig-aga/minimig_sram_bridge.v \
		minimig-aga/minimig_syscontrol.v \
		minimig-aga/userio.v \
        misc/hid.v \
        misc/mcu_spi.v \
        misc/osd_u8g2.v \
        misc/sd_card.v \
        misc/sd_rw.v \
        misc/sdcmd_ctrl.v \
        misc/sysctrl.v \
        misc/video_analyzer.v \
        tang/nano20k/flash_dspi.v \
        tang/nano20k/gowin_clkdiv/gowin_clkdiv.v \
        tang/nano20k/gowin_dpb/sector_dpram.v \
        tang/nano20k/gowin_rpll/pll_142m.v \
        tang/nano20k/sdram.v \
        tang/nano20k/top_lcd.sv

all: top.fs

top.fs: top.pack
	$(GOWINPACK) -d $(DEVICE_GOWINPACK) -o $@ $^

top.pack: top.json
	$(NEXTPNR) -l top_nextpnr.log --json $^ --write $@ --device $(DEVICE_NEXTPNR) --freq 27 --vopt family=$(FAMILY_NEXTPNR) --vopt cst=$(CONSTRAINTS)

top.json:
	$(YOSYS) $(MACRO_YOSYS) -ql top.log -p "read_verilog -sv $(SOURCES); synth_gowin -json top.json -top top" $(SOURCES)

prog: top.fs
	openFPGALoader -b tangnano $^

flash: top.fs
	openFPGALoader -b tangnano -f $^

reset:
	openFPGALoader --reset

clean:
	rm -f *.json *.fs *.pack

.PHONY: clean prog flash reset
