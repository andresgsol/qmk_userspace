Bootloader mode: FN+B or FN+V. I'm not sure actually.

Support for the Drop ALT V1 (`massdrop/alt`) was removed from the main QMK branch in late 2024. To compile for it, I need to check out an old version of `qmk_firmware`. Moreover, I also cannot flash it with usual `qmk flash`. I need to use mdloader.

Here's the full compiling and flashing recipe:
```
git clone https://github.com/Massdrop/mdloader.git ~/mdloader
make -C ~/mdloader && sudo usermod -a -G dialout $USER
git -C ~/qmk_firmware fetch --tags && git -C ~/qmk_firmware checkout 0.26.11
git -C ~/qmk_firmware submodule update --init --recursive
ln -s ~/qmk_userspace/keyboards/massdrop/alt/keymaps/mykeymap ~/qmk_firmware/keyboards/massdrop/alt/keymaps/mykeymap
qmk compile -kb massdrop/alt -km mykeymap && \
~/mdloader/build/mdloader --first --download ~/qmk_firmware/.build/massdrop_alt_mykeymap.bin --restart
```