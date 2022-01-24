project('lua', 'c', version: '5.4.3',
    default_options : [
        'c_std=gnu99',
        'default_library=static'
    ])
# Do not use c99 for c standard as it will produce a wrong build because
# functions like localtime_r will be not declared and C will assume they
# return an integer instead of a pointer and on 64bit this is horribly
# wrong.
# The gnu99 standard ensure that __USE_POSIX is defined (among other
# things) to get declaration of functions like localtime_r.

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
elif host_machine.system() == 'linux'
    lua_cflags += '-DLUA_USE_LINUX'
    readline = cc.find_library('readline', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_READLINE'
        lua_deps += readline
    endif
    lua_deps += cc.find_library('dl')
elif host_machine.system() in ['freebsd', 'openbsd', 'netbsd']
    lua_cflags += '-DLUA_USE_LINUX'
    readline = cc.find_library('edit', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_READLINE'
        lua_deps += readline
    endif
elif host_machine.system() == 'sunos'
    lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-D_REENTRANT']
    lua_deps += cc.find_library('dl')
elif host_machine.system() == 'darwin'
    lua_cflags += '-DLUA_USE_MACOSX'
    readline = cc.find_library('readline', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_READLINE'
        lua_deps += readline
    endif
elif host_machine.system() in ['openbsd', 'netbsd', 'dragonfly']
    lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN']
endif

lua_deps += cc.find_library('m', required : false)

install_headers(
    'src/lua.h',
    'src/luaconf.h',
    'src/lualib.h',
    'src/lauxlib.h',
    'src/lua.hpp',
)

subdir('src')
