cmake_minimum_required(VERSION 2.6)
project(brcm_patchram C)

# Override some CMAKE defaults for our own purposes...
set(EXECUTABLE_OUTPUT_PATH deploy)

# Check for BlueZ support
find_package(PkgConfig REQUIRED)
# We want, but do NOT require BlueZ support...
pkg_check_modules (BLUEZ bluez)

# Add our binaries we explicitly build regardless of what all is there...
add_executable(brcm_patchram_plus brcm_patchram_plus.c)
add_executable(brcm_patchram_plus_h5 brcm_patchram_plus_h5.c)

if (BLUEZ_FOUND)
	# Only build this if we find BlueZ's support layer is present for
	# the build.
	add_executable(brcm_patchram_plus_usb brcm_patchram_plus_usb.c)
	target_link_libraries(brcm_patchram_plus_usb ${BLUEZ_LIBRARIES})
	set(BRCM_USB_PATCHRAM brcm_patchram_plus_usb)
endif (BLUEZ_FOUND)

# Set up install rules...
install(TARGETS brcm_patchram_plus brcm_patchram_plus_h5 ${BRCM_USB_PATCHRAM} DESTINATION bin)

