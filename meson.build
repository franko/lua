project('lua', 'c', version: '5.2.4',
    default_options : [
        'c_std=c99',
        'default_library=static'
    ])

cc = meson.get_compiler('c')

lua_deps = []
lua_cflags = ['-DLUA_COMPAT_ALL']

lua_library_name = 'lua'

if host_machine.system() == 'windows'
    if get_option('default_library') != 'static'
        lua_cflags += '-DLUA_BUILD_AS_DLL'
        lua_library_name = 'lua52'
    endif
elif host_machine.system() == 'linux'
    lua_cflags += '-DLUA_USE_LINUX'
    readline = cc.find_library('readline')
    if not readline.found()
        error('readline library not found')
    endif
    lua_deps += readline
elif host_machine.system() == 'sunos'
   lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-D_REENTRANT']
elif host_machine.system() == 'darwin'
    lua_cflags += ['-DLUA_USE_MACOSX']
elif host_machine.system() in ['freebsd', 'openbsd', 'netbsd', 'dragonfly']
    lua_cflags += ['-DLUA_USE_LINUX']
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
