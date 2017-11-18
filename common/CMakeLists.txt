# 3 june 2016

list(APPEND _LIBUI_SOURCES
	common/areaevents.cpp
	common/control.cpp
	common/debug.cpp
	common/matrix.cpp
	common/shouldquit.cpp
	common/userbugs.cpp
)
set(_LIBUI_SOURCES ${_LIBUI_SOURCES} PARENT_SCOPE)

list(APPEND _LIBUI_INCLUDEDIRS
	common
)
set(_LIBUI_INCLUDEDIRS ${_LIBUI_INCLUDEDIRS} PARENT_SCOPE)
