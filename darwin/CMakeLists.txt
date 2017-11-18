# 3 june 2016

list(APPEND _LIBUI_SOURCES
	darwin/alloc.mm
	darwin/area.mm
	darwin/areaevents.mm
	darwin/autolayout.mm
	darwin/box.mm
	darwin/button.mm
	darwin/checkbox.mm
	darwin/colorbutton.mm
	darwin/combobox.mm
	darwin/control.mm
	darwin/datetimepicker.mm
	darwin/debug.mm
	darwin/draw.mm
	darwin/drawtext.mm
	darwin/editablecombo.mm
	darwin/entry.mm
	darwin/fontbutton.mm
	darwin/form.mm
	darwin/grid.mm
	darwin/group.mm
	darwin/image.mm
	darwin/label.mm
	darwin/main.mm
	darwin/map.mm
	darwin/menu.mm
	darwin/multilineentry.mm
	darwin/progressbar.mm
	darwin/radiobuttons.mm
	darwin/scrollview.mm
	darwin/separator.mm
	darwin/slider.mm
	darwin/spinbox.mm
	darwin/stddialogs.mm
	darwin/tab.mm
	darwin/text.mm
	darwin/util.mm
	darwin/window.mm
	darwin/winmoveresize.mm
)
set(_LIBUI_SOURCES ${_LIBUI_SOURCES} PARENT_SCOPE)

list(APPEND _LIBUI_INCLUDEDIRS
	darwin
)
set(_LIBUI_INCLUDEDIRS _LIBUI_INCLUDEDIRS PARENT_SCOPE)

set(_LIBUINAME libui PARENT_SCOPE)
if(NOT BUILD_SHARED_LIBS)
	set(_LIBUINAME libui-temporary PARENT_SCOPE)
endif()
# thanks to Mr-Hide in irc.freenode.net/#cmake
macro(_handle_static)
	set_target_properties(${_LIBUINAME} PROPERTIES
		ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
	set(_aname $<TARGET_FILE:${_LIBUINAME}>)
	set(_lname libui-combined.list)
	set(_oname libui-combined.o)
	add_custom_command(
		OUTPUT ${_oname}
		COMMAND
			nm -m ${_aname} | sed -E -n "'s/^[0-9a-f]* \\([A-Z_]+,[a-z_]+\\) external //p'" > ${_lname}
		COMMAND
			ld -exported_symbols_list ${_lname} -r -all_load ${_aname} -o ${_oname}
		COMMENT "Removing hidden symbols")
	add_library(libui STATIC ${_oname})
	# otherwise cmake won't know which linker to use
	set_target_properties(libui PROPERTIES
		LINKER_LANGUAGE CXX)
	set(_aname)
	set(_lname)
	set(_oname)
endmacro()

set(_LIBUI_LIBS
	objc "-framework Foundation" "-framework AppKit"
PARENT_SCOPE)
