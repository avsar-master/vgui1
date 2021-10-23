set( PROJECT_NAME vgui )

add_sources(
    App.cpp
    Bitmap.cpp
    BitmapTGA.cpp
    Border.cpp
    BorderLayout.cpp
    BorderPair.cpp
    BuildGroup.cpp
    Button.cpp
    ButtonGroup.cpp
    CheckButton.cpp
    Color.cpp
    ConfigWizard.cpp
    Cursor.cpp
    DataInputStream.cpp
    Desktop.cpp
    DesktopIcon.cpp
    EditPanel.cpp
    EtchedBorder.cpp
    FileInputStream.cpp
    FlowLayout.cpp
    FocusNavGroup.cpp
    Font.cpp
    Frame.cpp
    GridLayout.cpp
    HeaderPanel.cpp
    Image.cpp
    ImagePanel.cpp
    IntLabel.cpp
    Label.cpp
    Layout.cpp
    LineBorder.cpp
    ListPanel.cpp
    LoweredBorder.cpp
    Menu.cpp
    MenuItem.cpp
    MenuSeparator.cpp
    MessageBox.cpp
    MiniApp.cpp
    Panel.cpp
    PopupMenu.cpp
    ProgressBar.cpp
    RadioButton.cpp
    RaisedBorder.cpp
    Scheme.cpp
    ScrollBar.cpp
    ScrollPanel.cpp
    Slider.cpp
    StackLayout.cpp
    String.cpp
    Surface.cpp
    SurfaceBase.cpp
    TablePanel.cpp
    TabPanel.cpp
    TaskBar.cpp
    TextEntry.cpp
    TextGrid.cpp
    TextImage.cpp
    TextPanel.cpp
    ToggleButton.cpp
    TreeFolder.cpp
    vgui.cpp
    WizardPanel.cpp
)

if( WIN32 )
	add_sources(
		win32/App.cpp
        win32/Cursor.cpp
        win32/fileimage.cpp
        win32/fileimage.h
        win32/Font.cpp
        win32/Surface.cpp
        win32/vfontdata.cpp
        win32/vfontdata.h
        win32/vgui_win32.h
        win32/vgui.cpp
    )
elseif( UNIX )
    add_sources(
        linux/App.cpp
        linux/Cursor.cpp
        linux/fileimage.cpp
        linux/fileimage.h
        linux/Font.cpp
        linux/Surface.cpp
        linux/utlmemory.h
        linux/utlrbtree.h
        linux/vfontdata.cpp
        linux/vfontdata.h
        linux/vgui_linux.h
        linux/vgui.cpp
    )
elseif( OSX )
    add_sources(
        osx/App.cpp
        osx/Cursor.cpp
        osx/fileimage.cpp
        osx/fileimage.h
        osx/Font.cpp
        osx/Surface.cpp
        osx/utlmemory.h
        osx/utlrbtree.h
        osx/vfontdata.cpp
        osx/vfontdata.h
        osx/vgui_osx.h
        osx/vgui.cpp
    )
else()
    MESSAGE( FATAL_ERROR "Are you serious? I can't find OS" )
endif()

preprocess_sources()

add_library( ${PROJECT_NAME} SHARED ${PREP_SRCS} )

target_include_directories( ${PROJECT_NAME} PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}
	${SRC_DIR}/utils/vgui/include
)

target_compile_definitions( ${PROJECT_NAME} PRIVATE
	${SHARED_DEFS}
)

set( PROJECT_LIBS )

#Link with dependencies
target_link_libraries( ${PROJECT_NAME} 
	${PROJECT_LIBS}
)

#CMake places libraries in /Debug or /Release on Windows, so explicitly set the paths for both.
#On Linux, it uses LIBRARY_OUTPUT_DIRECTORY
set_target_properties( ${PROJECT_NAME} PROPERTIES
	LIBRARY_OUTPUT_DIRECTORY "${ENGINE_BASE_PATH}"
	RUNTIME_OUTPUT_DIRECTORY_DEBUG "${ENGINE_BASE_PATH}"
	RUNTIME_OUTPUT_DIRECTORY_RELEASE "${ENGINE_BASE_PATH}"
)

set( PROJECT_LINK_FLAGS )

set_target_properties( ${PROJECT_NAME} PROPERTIES
	COMPILE_FLAGS "${LINUX_32BIT_FLAG}"
	LINK_FLAGS "${SHARED_ENGINE_LINKER_FLAGS} ${PROJECT_LINK_FLAGS} ${LINUX_32BIT_FLAG}"
)
	
#No lib prefix
set_target_properties( ${PROJECT_NAME} PROPERTIES PREFIX "" )

#Create filters
create_source_groups( "${CMAKE_SOURCE_DIR}" )

clear_sources()

