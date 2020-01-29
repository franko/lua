project('lua', 'c', version: '5.1.5', default_options : ['c_std=c89'])

cc = meson.get_compiler('c')

lua_deps = []
lua_cflags = []
lua_dll_cflags = []
if host_machine.system() == 'linux'
    lua_deps += dependency('readline')
    lua_cflags += '-DLUA_USE_LINUX'
elif host_machine.system() == 'windows'
    if get_option('default_library') != 'static'
        lua_dll_cflags += '-DLUA_BUILD_AS_DLL'
    endif
endif

lua_deps += cc.find_library('m', required : false)
lua_deps += cc.find_library('dl', required : false)

# agg_includes = include_directories('include')
# install_subdir('include', install_dir : 'include/agg2', strip_directory : true)

subdir('src')
