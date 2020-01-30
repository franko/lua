project('lua', 'c', version: '5.1.5', default_options : ['c_std=c89'])

lua_version_suffix = '51'

cc = meson.get_compiler('c')

lua_deps = []
lua_cflags = []
lua_dll_cflags = []

if host_machine.system() == 'linux'
    readline = cc.find_library('readline')
    if not readline.found()
        error('readline library not found')
    endif
    lua_deps += readline
    lua_cflags += '-DLUA_USE_LINUX'
    lua_dll_name = 'lua'
    lua_dll_prefix = 'lib'
elif host_machine.system() == 'windows'
    lua_dll_cflags += '-DLUA_BUILD_AS_DLL'
    lua_dll_name = 'lua51'
    lua_dll_prefix = ''
endif

lua_deps += cc.find_library('m', required : false)
lua_deps += cc.find_library('dl', required : false)

install_headers(
    'src/lua.h',
    'src/luaconf.h',
    'src/lualib.h',
    'src/lauxlib.h',
    'etc/lua.hpp',
)

subdir('src')
