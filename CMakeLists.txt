cmake_minimum_required(VERSION 3.7)

project(onscripter-en)

find_package(SDL2 CONFIG REQUIRED)
find_package(SDL2_mixer CONFIG REQUIRED)
find_package(SDL2_ttf CONFIG REQUIRED)
find_package(SDL2_image CONFIG REQUIRED)
find_package(unofficial-smpeg2 CONFIG REQUIRED) 
find_package(BZip2 REQUIRED)
find_package(Vorbis CONFIG REQUIRED)

option(ON_USE_LUA "Enable support for LUA" FALSE)

set(ON_VIDEO_CODEC "MPEG" CACHE STRING "Video decoder used to play cutscenes")
set_property(CACHE ON_VIDEO_CODEC PROPERTY STRINGS MPEG AVI MAD)

set(ON_SOURCES
	"AnimationInfo.cpp"
	"DirPaths.cpp"
	"DirectReader.cpp"
	"DirtyRect.cpp"
	"Encoding.cpp"
	"FontInfo.cpp"
	"Layer.cpp"
	"NsaReader.cpp"
	"ONScripterLabel.cpp"
	"ONScripterLabel_animation.cpp"
	"ONScripterLabel_command.cpp"
	"ONScripterLabel_effect.cpp"
	"ONScripterLabel_effect_breakup.cpp"
	"ONScripterLabel_effect_cascade.cpp"
	"ONScripterLabel_effect_trig.cpp"
	"ONScripterLabel_event.cpp"
	"ONScripterLabel_file.cpp"
	"ONScripterLabel_file2.cpp"
	"ONScripterLabel_image.cpp"
	"ONScripterLabel_rmenu.cpp"
	"ONScripterLabel_sound.cpp"
	"ONScripterLabel_text.cpp"
	"SarReader.cpp"
	"ScriptHandler.cpp"
	"ScriptParser.cpp"
	"ScriptParser_command.cpp"
	"graphics_altivec.cpp"
	"graphics_mmx.cpp"
	"graphics_routines.cpp"
	"graphics_sse2.cpp"
	"onscripter.cpp"
	"resize_image.cpp"
	"sjis2utf16.cpp"
)

if(ON_VIDEO_CODEC EQUAL "AVI")
	set(APPEND ON_SOURCES "AVIWrapper.cpp")
elseif(ON_VIDEO_CODEC EQUAL "MAD")
	set(APPEND ON_SOURCES "MadWrapper.cpp")
endif()

add_executable(onscripter-en ${ON_SOURCES})
target_compile_definitions(onscripter-en PRIVATE WIN32 USE_MESSAGEBOX USE_OGG_VORBIS)
target_link_libraries(onscripter-en
	SDL2::SDL2
	SDL2::SDL2main
	SDL2_mixer::SDL2_mixer
	SDL2_ttf::SDL2_ttf
	SDL2_image::SDL2_image
	unofficial::smpeg2::smpeg2
	BZip2::BZip2
	Vorbis::vorbis
	Vorbis::vorbisfile
)
