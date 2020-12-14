project('lua', 'c', version: '5.2.4',
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
lua_cflags = ['-DLUA_COMPAT_ALL']
lua_shared_cflags = []
lua_library_name = 'lua'
lua_library_prefix = 'lib'

if host_machine.system() == 'windows'
    lua_shared_cflags += '-DLUA_BUILD_AS_DLL'
    lua_library_name = 'lua52'
    lua_library_prefix = ''
elif host_machine.system() in ['linux', 'freebsd']
    readline = cc.find_library('readline', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_LINUX'
        lua_deps += readline
    else
        # enable all Linux flags ecept LUA_USE_READLINE
        lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-DLUA_USE_STRTODHEX', '-DLUA_USE_AFORMAT', '-DLUA_USE_LONGLONG']
    endif
elif host_machine.system() == 'sunos'
   lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-D_REENTRANT']
elif host_machine.system() == 'darwin'
    readline = cc.find_library('readline', required: false)
    if readline.found() and get_option('use_readline')
        lua_cflags += '-DLUA_USE_MACOSX'
        lua_deps += readline
    else
        # enable all flags except LUA_USE_READLINE. Actually the same of Linux.
        lua_cflags += ['-DLUA_USE_POSIX', '-DLUA_USE_DLOPEN', '-DLUA_USE_STRTODHEX', '-DLUA_USE_AFORMAT', '-DLUA_USE_LONGLONG']
    endif
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
