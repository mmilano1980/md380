
all: wrapped
clean:
	rm -f *~ *.bin *.img

unwrapped.bin:
	cd ../../firmware && make D002.032.bin
	../../md380-fw --unwrap ../../firmware/D002.032.bin unwrapped.img

patches: unwrapped.bin
	../../md380-gfx --firmware=unwrapped.img --gfx=0x80f9ca8-poc.ppm write
	./patch.py

wrapped: patches
	../../md380-fw --wrap prom-public.img prom-public.bin
	../../md380-fw --wrap prom-private.img prom-private.bin
	../../md380-fw --wrap experiment.img experiment.bin


flash: patches
	stm32-dfu erase
	sleep 25
	stm32-dfu writeflash ~/jailbreak.bin
	stm32-dfu write experiment.img 0x0800C000
