project('lua', 'c', version: '5.4.0', default_options : ['c_std=gnu99'])

cc = meson.get_compiler('c')

lua_deps = []
lua_cflags = ['-DLUA_COMPAT_5_3']
lua_shared_cflags = []

lua_library_name = 'lua'
lua_library_prefix = 'lib'

if host_machine.system() == 'windows'
    lua_shared_cflags += '-DLUA_BUILD_AS_DLL'
    lua_library_name = 'lua54'
    lua_library_prefix = ''
elif host_machine.system() in ['linux', 'freebsd', 'darwin']
    # For Freebsd and MacOSX LUA_USE_LINUX can be used.
    # LUA_USE_MACOSX is equivalent to LUA_USE_LINUX.
    lua_cflags += '-DLUA_USE_LINUX'
    readline = cc.find_library('readline', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_READLINE'
        lua_deps += readline
    endif
elif host_machine.system() == 'sunos'
   lua_shared_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-D_REENTRANT']
elif host_machine.system() in ['openbsd', 'netbsd', 'dragonfly']
    lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN']
endif

lua_deps += cc.find_library('m', required : false)
lua_deps += cc.find_library('dl', required : false)

install_headers(
    'src/lua.h',
    'src/luaconf.h',
    'src/lualib.h',
    'src/lauxlib.h',
    'src/lua.hpp',
)

subdir('src')

