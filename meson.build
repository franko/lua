project('lua', 'c', version: '5.1.5', default_options : ['c_std=c89'])

lua_version_suffix = '51'

cc = meson.get_compiler('c')

lua_deps = []
lua_cmd_deps = []
lua_cflags = []
lua_shared_cflags = []

if host_machine.system() == 'linux'
    readline = cc.find_library('readline')
    if not readline.found()
        error('readline library not found')
    endif
    lua_cmd_deps += readline
    lua_cflags += '-DLUA_USE_LINUX'
    lua_library_name = 'lua'
    lua_shared_prefix = 'lib'
elif host_machine.system() == 'darwin'
    readline = cc.find_library('readline', required: false)
    lua_cmd_deps += readline
    lua_cflags += '-DLUA_USE_LINUX'
    lua_library_name = 'lua'
    lua_shared_prefix = 'lib'
elif host_machine.system() == 'windows'
    lua_shared_cflags += '-DLUA_BUILD_AS_DLL'
    lua_library_name = 'lua51'
    lua_shared_prefix = ''
else
    lua_library_name = 'lua'
    lua_shared_prefix = 'lib'
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
