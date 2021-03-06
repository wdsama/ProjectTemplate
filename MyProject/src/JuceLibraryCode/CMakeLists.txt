set(CurrLib JuceLibraryCode)

option (WITH_JUCE "build a juce library" OFF)
if (WITH_JUCE)
	find_package(JUCE)
	if (JUCE_FOUND)
		option(JUCE_MODULE_AVAILABLE_juce_core  "enable juce module juce_core" ON)
		option(JUCE_MODULE_AVAILABLE_juce_data_structures "enable juce module juce_data_structures" ON)
		option(JUCE_MODULE_AVAILABLE_juce_events "enable juce module juce_events" ON)
		option(JUCE_MODULE_AVAILABLE_juce_graphics "enable juce module juce_graphics" ON)
		option(JUCE_MODULE_AVAILABLE_juce_gui_basics "enable juce module juce_gui_basics" ON)
		option(JUCE_MODULE_AVAILABLE_juce_gui_extra "enable juce module juce_gui_extra" ON)
		option(JUCE_MODULE_AVAILABLE_juce_audio_basics "enable juce module juce_audio_basics" ON)
		option(JUCE_MODULE_AVAILABLE_juce_audio_devices "enable juce module juce_audio_devices" OFF)
		option(JUCE_MODULE_AVAILABLE_juce_audio_formats "enable juce module juce_audio_formats" OFF)
		option(JUCE_MODULE_AVAILABLE_juce_audio_plugin_client "enable juce module juce_audio_plugin_client" OFF)
		option(JUCE_MODULE_AVAILABLE_juce_audio_processors "enable juce module juce_audio_processors" OFF)
		option(JUCE_MODULE_AVAILABLE_juce_audio_utils "enable juce module juce_audio_utils" OFF)
		foreach(dir ${JUCE_MODULES})
			if (JUCE_MODULE_AVAILABLE_${dir})
				list(APPEND ${CurrLib}_SOURCES ${JUCE_SOURCES_${dir}})
				list(APPEND ${CurrLib}_HEADERS ${JUCE_HEADERS_${dir}})
			endif (JUCE_MODULE_AVAILABLE_${dir})
		endforeach(dir ${JUCE_MODULES})


		# build a static library
		add_library(${CurrLib} STATIC ${${CurrLib}_SOURCES} ${${CurrLib}_HEADERS})
		set_target_properties(${CurrLib} PROPERTIES CLEAN_DIRECT_OUTPUT 1)

		foreach(dir ${JUCE_MODULES})
			if (JUCE_MODULE_AVAILABLE_${dir})
			source_group("${dir}" FILES JUCE_SOURCES_${dir} JUCE_HEADERS_${dir})
			endif (JUCE_MODULE_AVAILABLE_${dir})
		endforeach(dir ${JUCE_MODULES})

		# set output name
		set_target_properties(${CurrLib} PROPERTIES OUTPUT_NAME "${CurrLib}")
		set_target_properties(${CurrLib} PROPERTIES PREFIX "lib")

		# copy platform config file
		configure_file(${CMAKE_SOURCE_DIR}/MyProject/src/JuceLibraryCode/AppConfig.h.in ${CMAKE_BINARY_DIR}/incl/AppConfig.h)
		configure_file(${CMAKE_SOURCE_DIR}/MyProject/src/JuceLibraryCode/JuceHeader.h.in ${CMAKE_BINARY_DIR}/incl/JuceHeader.h)
		include_directories(${CMAKE_BINARY_DIR}/incl)
	endif (JUCE_FOUND)
endif (WITH_JUCE)
